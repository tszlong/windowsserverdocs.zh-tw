---
title: Active Directory 同盟服務和憑證金鑰規格屬性資訊
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 36555336b158fdf7cfaa400f66b9deecbff49c1b
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519687"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS 和憑證 KeySpec 屬性資訊
金鑰規格（"KeySpec"）是與憑證和金鑰相關聯的屬性。 它會指定與憑證相關聯的私密金鑰是否可以用於簽署、加密或兩者。

不正確的 KeySpec 值可能會導致 AD FS 和 Web 應用程式 Proxy 錯誤，例如：


- 無法建立與 AD FS 或 Web 應用程式 Proxy 的 SSL/TLS 連線，且未記錄任何 AD FS 事件（但可能會記錄 SChannel 36888 和36874事件）
- 無法在 AD FS 或 WAP 以表單為基礎的驗證頁面上登入，頁面上不會顯示任何錯誤訊息。

您可能會在事件記錄檔中看到下列內容：

```
Log Name:   AD FS Tracing/Debug
Source: AD FS Tracing
Date:   2/12/2015 9:03:08 AM
Event ID:   67
Task Category: None
Level:  Error
Keywords:   ADFSProtocol
User:   S-1-5-21-3723329422-3858836549-556620232-1580884
Computer:   ADFS1.contoso.com
Description:
Ignore corrupted SSO cookie.
```

## <a name="what-causes-the-problem"></a>造成問題的原因
KeySpec 屬性會識別 Microsoft CryptoAPI （CAPI）從 Microsoft 舊版密碼編譯儲存提供者（CSP）所產生或取出的金鑰如何使用。

KeySpec 值**1**（或**AT_KEYEXCHANGE**）可用於簽署和加密。  值為**2**或**AT_SIGNATURE**，僅用於簽署。

最常見的 KeySpec 錯誤設定是針對權杖簽署憑證以外的憑證使用2的值。

對於其金鑰是使用新一代密碼編譯（CNG）提供者所產生的憑證，沒有金鑰規格的概念，而且 KeySpec 值一律為零。

請參閱下面的如何檢查有效的 KeySpec 值。

### <a name="example"></a>範例
舊版 CSP 的範例是 Microsoft 增強型密碼編譯提供者。

Microsoft RSA CSP 金鑰 blob 格式包括分別**CALG_RSA_KEYX**或**CALG_RSA_SIGN**的演算法識別碼，以服務<strong>AT_KEYEXCHANGE * * 或 * * AT_SIGNATURE</strong>金鑰的要求。

RSA 金鑰演算法識別碼會對應至 KeySpec 值，如下所示

| 提供者支援的演算法| CAPI 呼叫的金鑰規格值 |
| --- | --- |
|CALG_RSA_KEYX：可用於簽署和解密的 RSA 金鑰| AT_KEYEXCHANGE （或 KeySpec = 1）|
CALG_RSA_SIGN：僅限 RSA 簽章金鑰 |AT_SIGNATURE （或 KeySpec = 2）|

## <a name="keyspec-values-and-associated-meanings"></a>KeySpec 值和相關聯的意義
以下是各種 KeySpec 值的意義：

|Keyspec 值|利用|建議的 AD FS 使用|
| --- | --- | --- |
|0|憑證是 CNG 憑證|僅限 SSL 憑證|
|1|若是舊版 CAPI （非 CNG）憑證，金鑰可用於簽署和解密|    SSL、權杖簽署、權杖解密、服務通訊憑證|
|2|若是舊版 CAPI （非 CNG）憑證，金鑰只能用於簽署|不建議|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>如何檢查憑證/金鑰的 KeySpec 值
若要查看憑證值，您可以使用**certutil**命令列工具。

以下是範例： **certutil – v – store my**。  這會將憑證資訊傾印到螢幕上。

![Keyspec cert](media/AD-FS-and-KeySpec-Property/keyspec1.png)

在 CERT_KEY_PROV_INFO_PROP_ID 會尋找兩件事：


1. **ProviderType：** 這表示憑證是否使用舊版的密碼編譯儲存提供者（CSP），或以較新的憑證新一代（CNG） api 為基礎的金鑰儲存提供者。  任何非零值表示舊版提供者。
2. **KeySpec：** 以下是 AD FS 憑證的有效 KeySpec 值：

   舊版 CSP 提供者（ProviderType 不等於0）：

   |AD FS 憑證用途|有效的 KeySpec 值|
   | --- | --- |
   |服務通訊|1|
   |權杖解密|1|
   |權杖簽署|1和2|
   |SSL|1|

   CNG 提供者（ProviderType = 0）：

   |AD FS 憑證用途|有效的 KeySpec 值|
   | --- | --- |
   |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>如何將憑證的 keyspec 變更為支援的值
變更 KeySpec 值不需要重新產生憑證，或由憑證授權單位單位重新發出。  您可以使用下列步驟，將完整憑證和私密金鑰從 PFX 檔案重新匯入至憑證存放區，藉以變更 KeySpec：


1. 首先，檢查並記錄現有憑證的私密金鑰許可權，以便在重新匯入之後，視需要重新設定它們。
2. 將包含私密金鑰的憑證匯出至 PFX 檔案。
3. 針對每個 AD FS 和 WAP 伺服器執行下列步驟
    1. 刪除憑證（從 AD FS/WAP 伺服器）
    2. 開啟提升許可權的 PowerShell 命令提示字元，並使用下列 Cmdlet 語法，在每個 AD FS 和 WAP 伺服器上匯入 PFX 檔案，並指定 AT_KEYEXCHANGE 值（適用于所有 AD FS 憑證用途）：
        1. C： \> certutil – importpfx certfile .pfx AT_KEYEXCHANGE
        2. 輸入 PFX 密碼
    3. 上述步驟完成後，請執行下列動作
        1. 檢查私密金鑰許可權
        2. 重新開機 adfs 或 wap 服務





