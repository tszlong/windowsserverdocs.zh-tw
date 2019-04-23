---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: 裝置註冊技術參考
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fac6437e9b6c3893064769a8279c2cf96cbc47d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833779"
---
>適用於：Windows Server 2016, Windows Server 2012 R2

# <a name="device-registration-technical-reference"></a>裝置註冊技術參考
Device Registration Service \(DRS\)是一種新的 Windows 服務，隨附於 Active Directory 同盟服務角色 Windows Server 2012 R2 上。  DRS 必須安裝並設定在 AD FS 伺服器陣列中的所有同盟伺服器上。  如需部署 DRS 的詳細資訊，請參閱 [使用裝置註冊服務設定同盟伺服器](https://technet.microsoft.com/library/dn486831.aspx)。  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>登錄裝置時所建立的 Active Directory 物件  
下列 Active Directory 物件會建立為裝置註冊服務的一部分。  
  
### <a name="device-registration-configuration"></a>裝置註冊設定  
裝置註冊設定會儲存在 Active Directory 樹系的設定命名內容中。 \(例如， **CN\=Device Registration Configuration，CN\=服務，< 組態\-命名\-內容 >**\)。 此物件是在為裝置註冊初始化 Active Directory 樹系時建立。  
  
裝置註冊設定包括下列項目：  
  
-   **簽發者金鑰**  
  
    用來發行與已登錄裝置相關聯的 X.509 憑證的公開和私密金鑰。  私密金鑰受 DKM 保護。  
  
-   **裝置註冊服務設定**  
  
    與裝置註冊服務相關的原則。  
  
### <a name="registered-devices-container"></a>登錄的裝置容器  
裝置物件容器會在 Active Directory 樹系中的其中一個網域下建立。  此物件容器會包含 Active Directory 樹系的所有裝置物件。  
  
根據預設，容器是在 AD FS 的相同網域中建立。  \(例如， **CN\=RegisteredDevices，DC\=< 預設\-命名\-內容 >**\)。當裝置註冊初始化 Active Directory 樹系時，會建立此物件。  
  
### <a name="registered-devices"></a>登錄的裝置  
裝置物件是 Active Directory 中的新的輕量型物件。  它們用來代表使用者、裝置與公司之間的關聯性。  裝置物件使用 AD FS 所簽署的憑證來錨定實體裝置至 Active Directory 中的邏輯裝置物件。  
  
登錄的裝置包括下列元素：  
  
-   **顯示名稱**  
  
    裝置的好記的名稱。  針對 Windows 裝置，這是電腦的主機名稱。  
  
-   **裝置識別碼**  
  
    裝置註冊伺服器所產生的 GUID。  
  
-   **憑證指紋**  
  
    搭配登錄的裝置使用的 X.509 憑證的憑證指紋。  
  
-   **OS 類型**  
  
    裝置上的作業系統類別。  
  
-   **OS 版本**  
  
    裝置上的作業系統版本。  
  
-   **已啟用**  
  
    指出是否已在 Active Directory 中啟用裝置的布林值。  僅允許已啟用的裝置存取服務。  
  
-   **預估上次使用時間**  
  
    裝置用於存取資源的大約時間。  為了限制複寫流量，這僅會每隔 14 天更新一次。  
  
-   **已註冊的擁有者**  
  
    安全性身分識別\(SID\)這部裝置加入工作地點的使用者。  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>AD FS\/DRS 伺服器 SSL 憑證撤銷檢查  
工作地點加入用戶端會檢查 AD FS 伺服器 SSL 憑證的有效性。  如果 AD FS 伺服器 SSL 憑證包含憑證撤銷清單\(CRL\)端點，用戶端必須能夠連線到指定來驗證憑證的端點。  
  
如果您使用測試環境和測試憑證授權單位\(CA\)來發行您的伺服器 SSL 憑證，則您可以選擇在您的 CA 所發行的伺服器憑證中包含 CRL 端點。  如此能讓工作地方聯結用戶端略過 CRL 檢查。  
  
> [!CAUTION]  
> 永不建議對生產系統這麼做  
  

