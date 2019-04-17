---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: "設定聯盟伺服器裝置登記服務與"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 511a039afd47cf7570fffdcaf17842e0eccc5683
ms.sourcegitcommit: 9278435cbfa8dbeb30d0557ed0d27832b154edd2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/09/2018
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>設定聯盟伺服器裝置登記服務與

>適用於： Windows Server 2012 R2

您可以讓裝置登記服務 \(DRS\) 聯盟伺服器上之後您完成程序,[執行「步驟 4：設定聯盟伺服器](https://technet.microsoft.com/library/dn303424.aspx)。 裝置登記服務提供順暢的第二個訓練機制因數驗證、持續單一 sign\ 上 \(SSO\) 和條件消費者需要存取權的資源公司存取。 如需 DRS 的詳細資訊，請查看[加入的任何裝置 SSO 和順暢第二個因數驗證在公司應用程式的地點](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>準備您的 Active Directory 樹系支援的裝置  
  
> [!NOTE]  
> 這是您準備 Active Directory 樹系支援的裝置必須執行階段 one\ 作業。 您必須以企業的系統管理員權限登入並 Active Directory 樹系必須完成此程序的 Windows Server 2012 R2 結構描述。  
>   
> 此外，DRS，就需要至少一個通用伺服器森林根網域中。 通用伺服器，才能執行初始化 \-ADDeviceRegistration 和 AD FS 進行驗證時。 AD FS 初始化 in\ 記憶體的代表項 DRS 組態上的物件每個驗證要求，如果 DRS 組態物件上找不 DC 目前網域中，針對 DRS 物件已提供期間初始化 \-ADDeviceRegistration GC 嘗試要求。  
  
#### <a name="to-prepare-the-active-directory-forest"></a>準備 Active Directory 樹系  
  
1.  在您聯盟的伺服器，開放的 Windows PowerShell 命令視窗和類型：  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  當 ServiceAccountName 的提示，請輸入當做服務 account AD fs 服務 account 的名稱。  如果是 gMSA 帳號，輸入中的帳號**domain\\accountname$**格式。 適用於核對，使用的格式**domain\\accountname**。  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>聯盟伺服器發電廠節點上讓裝置登記服務  
  
> [!NOTE]  
> 您必須登入以完成此程序的網域系統管理員權限。  
  
#### <a name="to-enable-device-registration-service"></a>若要讓裝置登記服務  
  
1.  在您聯盟的伺服器，開放的 Windows PowerShell 命令視窗和類型：  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  重複此步驟，AD FS 陣列中每個聯盟發電廠節點上。  
  
## <a name="enable-seamless-second-factor-authentication"></a>讓順暢第二個因素驗證  
順暢第二個因數驗證是 enhancement 中提供的外部裝置，嘗試存取它們公司資源和應用程式存取保護新增層級 AD FS。 當地點加入個人裝置，在已知' 裝置和系統管理員可以使用此資訊來資源磁碟機條件存取和門存取。  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>若要讓順暢的第二個因數驗證、持續單一 sign\ 上 \(SSO\) 和條件存取加入的工作地點裝置  
  
1.  AD FS 管理主控台中，瀏覽至驗證原則。 選取 [編輯全球主要驗證。 選取 [讓裝置驗證旁邊的核取方塊，然後按一下 [確定]。  
  
## <a name="update-the-web-application-proxy-configuration"></a>更新 Web 應用程式的 Proxy 設定  
  
> [!IMPORTANT]  
> 您不需要應用程式網路 proxy 發行裝置登記服務。  裝置登記服務將可獲得應用程式網路 Proxy 之後可以聯盟的伺服器上。  您可能需要在完成之前讓裝置登記服務部署如果更新 Web 應用程式的 Proxy 設定此程序。  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>更新 Web 應用程式的 Proxy 設定  
  
1.  在您的網頁應用程式的 Proxy 伺服器開放的 Windows PowerShell 命令視窗中，輸入  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  出現提示時輸入認證，輸入的權限來聯盟伺服器的認證。  
  
## <a name="see-also"></a>也了 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署聯盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

