---
title: "Active Directory 同盟服務和憑證鍵規格屬性資訊"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a5307da5-02ff-4c31-80f0-47cb17a87272
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: db58fcce054f34c4b0a3f6725456badae9fd0468
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-and-certificate-keyspec-property-information"></a>AD FS 和憑證 KeySpec 屬性資訊
按鍵規格 (」 KeySpec 」) 是憑證與鍵相關聯的屬性。 它會指定憑證相關聯的私密金鑰可用來登入、 加密，或兩者。   

例如，不正確的 KeySpec 值可能會造成 AD FS 和 Web 應用程式 Proxy 錯誤：


- AD FS 事件登入 （但 SChannel 36888 和 36874 事件可能登入） 的建立 SSL 日 TLS 連接 AD FS 或應用程式網路 Proxy，失敗
- AD FS 或 WAP 登入失敗形成根據的驗證] 頁面上，以顯示在頁面上的任何錯誤訊息。

您可能會看到事件木頭中的動作：

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

## <a name="what-causes-the-problem"></a>問題的原因
KeySpec 方便的按鍵發生或擷取由 Microsoft CryptoAPI (CAPI)，從 Microsoft 舊版密碼編譯儲存提供者 (CSP)，可以使用方式。

KeySpec 值為**1**，或**AT_KEYEXCHANGE**，可用於簽署及加密。  為**2**，或**AT_SIGNATURE**，僅用來登入。

最常見的 KeySpec 錯誤設定所使用的值為 2 的簽署憑證權杖以外的憑證。  

對於使用密碼編譯下一代 (CNG) 提供者程式其鍵憑證，還有金鑰規格的概念，並 KeySpec 值一定會零。

了解如何檢查 KeySpec 有效值下方。 

### <a name="example"></a>範例
範例舊版 CSP 為 Microsoft 增強密碼編譯提供者。 

Microsoft RSA CSP 大型物件格式包含識別字演算法，請**CALG_RSA_KEYX**或**CALG_RSA_SIGN**，分別服務要求其中一個 * * AT_KEYEXCHANGE * * 或**AT_SIGNATURE**鍵。
  
RSA 演算法識別碼，如下所示對應至 KeySpec 值

| 支援的提供者演算法| 適用於 CAPI 通話鍵規格值 |
| --- | --- |
|可用來登入和解密 CALG_RSA_KEYX: RSA 鍵| AT_KEYEXCHANGE (或 KeySpec = 1 台)|
僅限金鑰的 CALG_RSA_SIGN: RSA 簽章 |AT_SIGNATURE (或 KeySpec = 2)|

## <a name="keyspec-values-and-associated-meanings"></a>KeySpec 值和相關的意義。
以下是各種 KeySpec 值的意義︰

|Keyspec 值。|表示|建議使用的 AD FS 使用|
| --- | --- | --- |
|0|憑證的 CNG 憑證|僅限 SSL 憑證|
|1|適用於傳統 CAPI (非 CNG) 憑證，按鍵可用來登入和解密|    SSL，預付碼簽章權杖解密，服務通訊憑證|
|2|適用於傳統 CAPI (非 CNG) 憑證，可以使用按鍵僅適用於登入|不建議|

## <a name="how-to-check-the-keyspec-value-for-your-certificates--keys"></a>若要查看您的憑證 KeySpec 值如何 / 下鍵
若要查看憑證值，您可以使用**certutil**命令列工具。  

以下是範例： **certutil-v – 儲存我**。  這會傾印畫面憑證的資訊。

![Keyspec 憑證](media/AD-FS-and-KeySpec-Property/keyspec1.png)

在 CERT_KEY_PROV_INFO_PROP_ID 尋找兩個項目：


1. **提供者類型：**這表示還是憑證使用舊版的密碼編譯儲存提供者 (CSP) 的金鑰儲存提供者以在較新的憑證下一代 (CNG) Api。  任何為零表示舊版的提供者。
2.  **KeySpec:** AD FS 憑證 KeySpec 有效值如下：

    舊版 CSP 提供者 （如提供者類型為 0 不相同）：
    
    |AD FS 憑證用途|有效 KeySpec 值|
    | --- | --- |
    |服務通訊|1|
    |權杖解密|1|
    |權杖登入|1 到 2|
    |SSL|1|

    CNG 提供者 (提供者類型 = 0):
    |AD FS 憑證用途|有效 KeySpec 值|
    | --- | --- |   
    |SSL|0|

## <a name="how-to-change-the-keyspec-for-your-certificate-to-a-supported-value"></a>如何變更您的憑證 keyspec 支援的值
變更 KeySpec 值，不需要的憑證會重新發生，或重新是發行憑證授權單位。  變更 KeySpec 重新匯入的完整憑證和 PFX 檔案從私密金鑰憑證存放區，使用下列步驟：


1. 首先，查看並錄製現有的憑證的私人按鍵權限，它們可以重新設定必要之後重新匯入。
2. 匯出包括私密金鑰檔案 PFX 憑證。
3. 執行下列步驟針對每個 AD FS 和 WAP 伺服器
    1. Delete 憑證 (從 AD FS 日 WAP 伺服器)
    2. 打開提升權限的 PowerShell 命令提示字元及匯入 PFX 檔案，使用下列 cmdlet 語法，指定 AT_KEYEXCHANGE 值 （適用於所有 AD FS 憑證目的） 每個 AD FS 和 WAP 伺服器上：
        1. C:\ > certutil-importpfx certfile.pfx AT_KEYEXCHANGE
        2. 輸入 PFX 密碼
    3. 上述完成之後，執行下列動作
        1. 檢查私人按鍵權限
        2. 重新開機 adfs 或 wap 服務





