---
title: Active Directory Federation Services 和憑證金鑰規格屬性資訊
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b0d08f678e9e612bb0ce9cc38d254564bd9b2f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444096"
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS 和憑證 KeySpec 屬性資訊
金鑰規格 ("KeySpec 」) 是憑證與金鑰相關聯的屬性。 它會指定與憑證相關聯的私用金鑰是否可以用於簽署、 加密，或兩者。   

這類 KeySpec 值不正確可能會導致 AD FS 和 Web 應用程式 Proxy 錯誤：


- 若要建立 AD FS 或 Web Application Proxy 的 SSL/TLS 連線與記錄 （雖然可能會記錄 36888 和 36874 的安全通道事件） 的 AD FS 事件不失敗
- 在 AD FS 或 WAP 登入失敗表單型的驗證 頁面上，並沒有顯示在頁面上的錯誤訊息。

您可能會看到下列事件記錄檔中：

    Log Name:      AD FS Tracing/Debug
    Source:        AD FS Tracing
    Date:          2/12/2015 9:03:08 AM
    Event ID:      67
    Task Category: None
    Level:         Error
    Keywords:      ADFSProtocol
    User:          S-1-5-21-3723329422-3858836549-556620232-1580884
    Computer:      ADFS1.contoso.com
    Description:
    Ignore corrupted SSO cookie.

## <a name="what-causes-the-problem"></a>造成此問題的原因
KeySpec 屬性會識別產生或擷取由 Microsoft CryptoAPI (CAPI)，從 Microsoft 傳統密碼編譯儲存體提供者 (CSP) 的金鑰用法。

KeySpec 值**1**，或**AT_KEYEXCHANGE**，可用來簽署和加密。  值為**2**，或**AT_SIGNATURE**，只用來簽署。

最常見 KeySpec 組態錯誤以外的權杖簽署憑證的憑證使用的值為 2。  

對於憑證的金鑰產生使用 Cryptography Next Generation (CNG) 提供者，沒有索引鍵的規格中，概念，和 KeySpec 值一律為零。

了解如何檢查有有效的 KeySpec 值以下。 

### <a name="example"></a>範例
舊版的 CSP 的範例是 Microsoft Enhanced Cryptographic Provider。 

Microsoft RSA CSP 金鑰 blob 格式包含演算法識別項，請**CALG_RSA_KEYX**或是**CALG_RSA_SIGN**，分別針對服務要求<strong>AT_KEYEXCHANGE * * 或 * * AT_簽章</strong>索引鍵。

RSA 金鑰演算法識別項對應至 KeySpec 值，如下所示

| 支援的提供者演算法| 金鑰規格 CAPI 呼叫的值 |
| --- | --- |
|CALG_RSA_KEYX:可以用於簽署及解密的 RSA 金鑰| AT_KEYEXCHANGE (或 KeySpec = 1)|
CALG_RSA_SIGN:金鑰的 RSA 簽章 |AT_SIGNATURE (或 KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>KeySpec 值和相關聯的意義
以下是各種 KeySpec 值的意義：

|Keyspec 值|方法|建議的 AD FS 使用|
| --- | --- | --- |
|0|憑證的 CNG 憑證|只使用 SSL 憑證|
|1|舊版的 CAPI (非 CNG) 憑證，金鑰可以用於簽署及解密|    SSL，權杖簽署、 權杖解密，服務通訊憑證|
|2|舊版的 CAPI (非 CNG) 憑證，可以使用的金鑰只能用於簽章|不建議使用|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>如何檢查 KeySpec 值，為您的憑證 / 金鑰
若要查看您可以使用的憑證值**certutil**命令列工具。  

以下是範例： **certutil – v – 儲存我**。  這會將傾印至畫面的憑證資訊。

![Keyspec 憑證](media/AD-FS-and-KeySpec-Property/keyspec1.png)

下兩件事 CERT_KEY_PROV_INFO_PROP_ID 外觀：


1. **提供者類型：** 這代表是否使用舊版的密碼編譯儲存體提供者 (CSP) 的憑證或金鑰儲存提供者基礎上較新的憑證 Next Generation (CNG) Api。  任何非零的值會指出舊版的提供者。
2. **KeySpec:** AD FS 憑證的有效 KeySpec 值如下：

   舊版的 CSP 提供者 (不等於 0 的 ProviderType):

   |AD FS 憑證用途|有效的 KeySpec 值|
   | --- | --- |
   |服務通訊|1|
   |權杖解密|1|
   |權杖簽署|1 和 2|
   |SSL|1|

   CNG 提供者 (提供者類型 = 0):

   |AD FS 憑證用途|有效的 KeySpec 值|
   | --- | --- |   
   |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>如何將您的憑證 keyspec 變更為支援的值
KeySpec 值變更時，不需要重新產生或重新發行憑證授權單位憑證。  可以變更 KeySpec 重新匯入完整的憑證和私密金鑰的 PFX 檔案從憑證存放區使用下列步驟：


1. 首先，檢查和記上現有的憑證私用金鑰的權限，使它們可以重新設定視之後重新匯入。
2. 匯出包括私密金鑰的 PFX 檔案的憑證。
3. 執行下列步驟，為每個 AD FS 和 WAP 伺服器
    1. 刪除憑證 (來自 AD FS / WAP 伺服器)
    2. 開啟提升權限的 PowerShell 命令提示字元，並匯入 PFX 檔案的每部 AD FS 和 WAP 伺服器上使用下列 cmdlet 語法，請指定 AT_KEYEXCHANGE 值 （這適用於所有的 AD FS 憑證用途）：
        1. C:\>certutil-importpfx certfile.pfx AT_KEYEXCHANGE
        2. 輸入 PFX 密碼
    3. 上述完成後，請執行下列
        1. 檢查私用金鑰的權限
        2. 重新啟動 adfs 或 wap 服務





