---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: 使用裝置註冊服務設定同盟伺服器
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f1367f03ea8a9ba96bfe4bae1c324deff92576f0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192264"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>使用裝置註冊服務設定同盟伺服器

您可以啟用 Device Registration Service \(DRS\)在同盟伺服器之後完成的程序[步驟 4:Configure a Federation Server](https://technet.microsoft.com/library/dn303424.aspx)。 裝置註冊服務提供上架機制，可無縫式次要因素驗證、 持續性的單一登\-上\(SSO\)，與需要存取公司的取用者的條件式存取資源。 如需 DRS 的詳細資訊，請參閱[從任何裝置加入工作地點網路提供 SSO 和無縫式第二個因素 Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>準備您的 Active Directory 樹系，以支援裝置  
  
> [!NOTE]  
> 這是一個\-時間來準備您的 Active Directory 樹系，以支援的裝置，您必須執行的作業。 您必須以企業系統管理員權限登入，且您的 Active Directory 樹系必須具有 Windows Server 2012 R2 結構描述，才能完成此程序。  
>   
> 此外，DRS 會需要您在樹系根網域中有至少一個通用類別目錄伺服器。 通用類別目錄伺服器，才能執行初始化\-ADDeviceRegistration 和 AD FS 進行驗證時。 AD FS 初始化在\-DRS 設定記憶體內部表示物件上每個驗證要求，並要求如果 DRS 組態物件無法在 DC 上找到目前網域中，會嘗試針對 DRS 物件已在其的 GC在初始化期間佈建\-ADDeviceRegistration。  
  
#### <a name="to-prepare-the-active-directory-forest"></a>若要準備 Active Directory 樹系  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並輸入：  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  當提示 ServiceAccountName，請輸入您選取做為服務帳戶，適用於 AD FS 服務帳戶的名稱。  如果是 gMSA 帳戶，請輸入中的帳戶**網域\\accountname$** 格式。 網域帳戶，使用格式**網域\\accountname**。  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>同盟伺服器陣列節點上啟用裝置註冊服務  
  
> [!NOTE]  
> 您必須登入具備網域系統管理員權限，才能完成此程序。  
  
#### <a name="to-enable-device-registration-service"></a>若要啟用 Device Registration Service  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並輸入：  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  重複此步驟中您的 AD FS 伺服器陣列中每個同盟伺服器陣列節點上...  
  
## <a name="enable-seamless-second-factor-authentication"></a>啟用無縫式次要因素驗證  
無縫式次要因素驗證是提供從嘗試存取它們的外部裝置的存取保護公司資源與應用程式的更的深層的 AD FS 中的增強功能。 當個人裝置加入工作地點時，它已成為 '已知' 的裝置，而且系統管理員可以使用這項資訊來驅動資源的條件式存取和限制存取權限。  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>若要啟用無縫式的第二個因素驗證，持續性的單一登入\-上\(SSO\)和已加入工作場所的裝置的條件式存取  
  
1.  在 AD FS 管理主控台中，瀏覽至 驗證原則。 選取 編輯全域主要驗證。 選取 啟用裝置驗證，旁邊的核取方塊，然後按一下 確定。  
  
## <a name="update-the-web-application-proxy-configuration"></a>更新 Web 應用程式 Proxy 組態  
  
> [!IMPORTANT]  
> 您不需要將裝置註冊服務發行至 Web 應用程式 Proxy。  一旦在同盟伺服器上啟用裝置註冊服務將可透過 Web Application Proxy。  若要完成此程序來更新 Web 應用程式 Proxy 組態，如果已啟用裝置註冊服務之前部署。  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>若要更新 Web 應用程式 Proxy 設定  
  
1.  在您 Web 應用程式 Proxy 伺服器上，開啟 Windows PowerShell 命令視窗並輸入  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  當提示認證，請輸入具有您的同盟伺服器的系統管理權限的帳戶的認證。  
  
## <a name="see-also"></a>另請參閱 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

