---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: "裝置登記技術參考"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fac6437e9b6c3893064769a8279c2cf96cbc47d6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
>適用於：Windows Server 2016、Windows Server 2012 R2

# <a name="device-registration-technical-reference"></a>裝置登記技術參考
裝置登記服務 \(DRS\) 是新的 Windows 服務中所包含的 Active Directory 同盟服務角色 Windows Server 2012 R2 上。  DRS 必須安裝和設定的所有 AD FS 陣列中聯盟伺服器上。  適用於部署 DRS 資訊，請查看[設定聯盟伺服器裝置登記服務與](https://technet.microsoft.com/library/dn486831.aspx)。  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>建立係裝置時的 active Directory 物件  
下列 Active Directory 物件的建成裝置登記服務的一部分。  
  
### <a name="device-registration-configuration"></a>裝置登記設定  
裝置登記設定會儲存在 Active Directory 樹系的組態命名操作。 \ (例如，**CN\ = 裝置登記組態 CN\ = 服務 < 操作-naming\ configuration\->**\)。 此物件會建立時 Active Directory 樹系 initialed 的裝置登記。  
  
裝置登記設定包括下列項目：  
  
-   **發行者鍵**  
  
    用來發行 X.509 憑證，且已裝置相關聯的公開和私人金鑰。  私密金鑰是 DKM 受保護。  
  
-   **裝置登記服務設定**  
  
    與裝置登記服務相關的原則。  
  
### <a name="registered-devices-container"></a>且已的裝置容器  
裝置物件的容器在其中一個網域中建立 Active Directory 樹系。  此物件的容器會包含所有的裝置的樹系 Active Directory 物件。  
  
根據預設，AD FS 相同的網域中建立容器。  \ (例如，**CN\ = RegisteredDevices，DC\ = < 操作-naming\ default\->**\)。此物件會建立時 Active Directory 樹系 initialed 的裝置登記。  
  
### <a name="registered-devices"></a>且已的裝置  
裝置物件的 Active Directory 中新的輕量減重物件。  它們用來表示之間的關係：使用者、裝置和公司。  裝置物件使用定位邏輯裝置中的物件 Active Directory 實體裝置，AD FS 簽署的憑證。  
  
且已的裝置包括下列項目：  
  
-   **顯示名稱**  
  
    裝置的易記名稱。  適用於 windows 的裝置，這是主機電腦的名稱。  
  
-   **裝置 Id**  
  
    GUID 所裝置登記伺服器。  
  
-   **憑證指紋**  
  
    憑證指紋 X.509 憑證，且已裝置搭配使用。  
  
-   **OS 類型**  
  
    作業系統類型的裝置上。  
  
-   **作業系統版本**  
  
    在裝置上的作業系統版本。  
  
-   **已支援**  
  
    布林值，表示裝置是否尚未在 Active Directory 中。  只有讓的裝置已獲授權存取服務。  
  
-   **大約上次的使用時間**  
  
    大約時間存取資源使用該裝置。  若要限制複寫流量，這才會更新一次每個 14 天。  
  
-   **且已擁有者**  
  
    安全性身分 \(SID\) 工作場所加入此裝置的使用者。  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>廣告 FS\ 日 DRS 伺服器 SSL 憑證撤銷檢查  
加入的工作地點 client 檢查有效的 AD FS 伺服器 SSL 憑證。  AD FS 伺服器 SSL 憑證包含撤銷的憑證清單 \(CRL\) 端點，如果 client 必須瑞曲之戰指定驗證憑證的端點。  
  
如果您正在使用的測試環境並測試憑證授權單位 \(CA\) 然後您可以選擇不包含 CA 所發行的伺服器憑證中的端點 CRL 發行伺服器 SSL 憑證。  這樣可讓地點加入 client 略過 CRL 檢查。  
  
> [!CAUTION]  
> 不建議這樣 production 系統  
  

