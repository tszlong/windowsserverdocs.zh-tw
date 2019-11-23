---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: 裝置註冊技術參考
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ab78a5847c52650f2a608dfc89e2001cc43153ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407353"
---
# <a name="device-registration-technical-reference"></a>裝置註冊技術參考
裝置註冊服務 \(DRS\) 是 Windows Server 2012 R2 上 Active Directory 同盟服務角色隨附的新 Windows 服務。  DRS 必須安裝並設定在 AD FS 伺服器陣列中的所有同盟伺服器上。  如需部署 DRS 的詳細資訊，請參閱 [使用裝置註冊服務設定同盟伺服器](https://technet.microsoft.com/library/dn486831.aspx)。  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>登錄裝置時所建立的 Active Directory 物件  
下列 Active Directory 物件會建立為裝置註冊服務的一部分。  
  
### <a name="device-registration-configuration"></a>裝置註冊設定  
裝置註冊設定會儲存在 Active Directory 樹系的設定命名內容中。 \(例如， **cn\=裝置註冊設定、cn\=Services、< Configuration\-命名\-內容**>\)。 此物件是在為裝置註冊初始化 Active Directory 樹系時建立。  
  
裝置註冊設定包括下列項目：  
  
-   **簽發者金鑰**  
  
    用來發行與已登錄裝置相關聯的 X.509 憑證的公開和私密金鑰。  私密金鑰受 DKM 保護。  
  
-   **裝置註冊服務設定**  
  
    與裝置註冊服務相關的原則。  
  
### <a name="registered-devices-container"></a>登錄的裝置容器  
裝置物件容器會在 Active Directory 樹系中的其中一個網域下建立。  此物件容器會包含 Active Directory 樹系的所有裝置物件。  
  
根據預設，容器是在 AD FS 的相同網域中建立。  \(例如， **CN\=RegisteredDevices，DC\=< 預設\-命名\-內容 >** \)。當 Active Directory 樹系在註冊裝置時，會建立此物件。  
  
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
  
-   **作業系統版本**  
  
    裝置上的作業系統版本。  
  
-   **已啟用**  
  
    指出是否已在 Active Directory 中啟用裝置的布林值。  僅允許已啟用的裝置存取服務。  
  
-   **估計上次使用時間**  
  
    裝置用於存取資源的大約時間。  為了限制複寫流量，這僅會每隔 14 天更新一次。  
  
-   **已註冊的擁有者**  
  
    將此裝置加入工作場所的使用者 \(SID\) 的安全性身分識別。  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>AD FS\/DRS 伺服器 SSL 憑證撤銷檢查  
工作地點加入用戶端會檢查 AD FS 伺服器 SSL 憑證的有效性。  如果 AD FS Server SSL 憑證包含 \(CRL\) 端點的憑證撤銷清單，則用戶端必須能夠連線到指定的端點來驗證憑證。  
  
如果您使用測試環境，以及 \(CA\) 的測試憑證授權單位單位來發行您的伺服器 SSL 憑證，則您可以選擇不要在 CA 所簽發的伺服器憑證中包含 CRL 端點。  如此能讓工作地方聯結用戶端略過 CRL 檢查。  
  
> [!CAUTION]  
> 永不建議對生產系統這麼做  
  

