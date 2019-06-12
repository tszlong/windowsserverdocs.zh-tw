---
title: 安裝憑證授權單位
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1774d235703bd75d810f2649cb8ed3f2f92622d5
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811602"
---
# <a name="install-the-certification-authority"></a>安裝憑證授權單位

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序安裝 Active Directory 憑證服務 (AD CS)，讓您可以註冊的伺服器憑證，來執行網路原則伺服器 (NPS)、 路由及遠端存取服務 (RRAS) 或兩者的伺服器。  
  
> [!IMPORTANT]  
> -   在安裝 Active Directory 憑證服務之前，您必須電腦命名為、 使用靜態 IP 位址，設定電腦和加入網域的電腦。 如需有關如何完成這些工作的詳細資訊，請參閱 Windows Server 2016[核心網路指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)。  
> -   若要執行此程序，安裝 AD CS 的電腦必須加入 Active Directory 網域服務 (AD DS) 安裝所在的網域。  
  
中的成員資格**Enterprise Admins**與根網域**Domain Admins**群組是要完成此程序，至少需要。  
  
> [!NOTE]  
> 使用 Windows PowerShell 執行此程序，開啟 Windows PowerShell 並輸入下列命令，，然後按 ENTER。   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> 已安裝 AD CS 之後，請輸入下列命令，然後按 ENTER。  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>若要安裝 Active Directory 憑證服務  

> [!TIP]
> 如果您想要使用 Windows PowerShell 來安裝 Active Directory 憑證服務，請參閱[Install-adcscertificationauthority](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcscertificationauthority?view=win10-ps) cmdlet 和選擇性參數。
  
1.  Enterprise Admins 群組和根網域的 Domain Admins 群組的成員身分登入。  
  
2.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能精靈] 隨即開啟。  
  
3.  在 [在您開始前]  中，按 [下一步]  。  
  
    > [!NOTE]  
    > 如果您先前在執行 [新增角色及功能精靈] 時已經選取 [預設略過這個頁面]  ，就不會顯示 [新增角色及功能精靈] 的 [在您開始前]  頁面。  
  
4.  在 [選取安裝類型]  中，確定已選取 [角色型或功能型安裝]  ，然後按 [下一步]  。  
  
5.  在 [選取目的地伺服器]  中，確定已選取 [從伺服器集區選取伺服器]  。 在 [伺服器集區]  中，確定已選取本機電腦。 按一下 [下一步]  。  
  
6.  在 **選取伺服器角色**，請在**角色**，選取**Active Directory 憑證服務**。 當系統提示您新增必要的功能時，按一下**將功能加入**，然後按一下**下一步**。  
  
7.  在 [**選取的功能**，按一下**下一步]** 。  
  
8.  在 [ **Active Directory 憑證服務**閱讀提供的資訊，然後按一下**下一步]** 。  
  
9. 在 [確認安裝選項]  中，按一下 [安裝]  。 在安裝過程中未關閉精靈。 安裝完成時，按一下**設定 Active Directory 憑證服務在目的地伺服器上**。 AD CS 設定精靈隨即開啟。 讀取認證資訊，並且如有需要提供 Enterprise Admins 群組的成員帳戶的認證。 按一下 [下一步]  。  
  
10. 在 [**角色服務**，按一下**憑證授權單位**，然後按一下**下一步]** 。  
  
11. 在 [**安裝類型**頁面上，確認**企業 CA**已選取，然後按一下**下一步]** 。  
  
12. 在 [**指定的 CA 類型**頁面上，確認**根 CA**已選取，然後按一下**下一步]** 。  
  
13. 在上**指定的私用的索引鍵類型**頁面上，確認**建立新的私密金鑰**已選取，然後按一下**下一步**。  
  
14. 在上**CA 的密碼編譯**頁面上，保留預設設定的 CSP (**RSA #Microsoft 軟體金鑰儲存提供者**) 和雜湊演算法 (**SHA2**)，並決定最佳金鑰字元長度，為您的部署的。 大型的金鑰字元長度可以提供最佳的安全性;不過，它們可能會影響伺服器效能，而且可能不相容的舊版應用程式。 建議您保留預設設定為 2048年。 按一下 [下一步]  。  
  
15. 在  **CA 名稱**頁面上，維持建議的 CA 一般名稱或變更的名稱，根據您的需求。 請確定您是特定 CA 名稱適用於您的命名慣例和用途，因為您已安裝 AD CS 之後，您無法變更 CA 名稱。 按一下 [下一步]  。  
  
16. 在 **有效期**頁面上，於**指定有效期間**，輸入的號碼，然後選取 時間值 （年、 月、 週或日）。 建議您使用預設設定 5 年。 按一下 [下一步]  。  
  
17. 在  **CA 資料庫**頁面上，於**指定資料庫位置**，指定憑證資料庫和憑證資料庫記錄檔的資料夾位置。 如果您指定的預設位置以外的位置，請確定資料夾受到使用存取控制清單 (Acl)，以防止未經授權的使用者或電腦存取 CA 資料庫和記錄檔。 按一下 [下一步]  。  
  
18. 在 **確認**，按一下**設定**來套用您的選項，然後按一下**關閉**。  
  


