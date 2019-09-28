---
title: 安裝憑證授權單位
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 024fc73c4ed089d81808cf44d7cfe8b01bfffaa0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406326"
---
# <a name="install-the-certification-authority"></a>安裝憑證授權單位

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式安裝 Active Directory 憑證服務（AD CS），以便向執行網路原則伺服器（NPS）、路由及遠端存取服務（RRAS）或兩者的伺服器註冊伺服器憑證。  
  
> [!IMPORTANT]  
> -   在安裝 Active Directory 憑證服務之前，您必須先將電腦命名為，並將電腦設定為使用靜態 IP 位址，並將電腦加入網域。 如需如何完成這些工作的詳細資訊，請參閱《 Windows Server 2016[核心網路指南》](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)。  
> -   若要執行此程式，您要安裝 AD CS 的電腦必須加入 Active Directory Domain Services （AD DS）安裝所在的網域。  
  
若要完成此程式，至少需要**Enterprise Admins**和根域的**domain admins**群組的成員資格。  
  
> [!NOTE]  
> 若要使用 Windows PowerShell 執行此程式，請開啟 Windows PowerShell 並輸入下列命令，然後按 ENTER。   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> 安裝 AD CS 之後，請輸入下列命令，然後按 ENTER。  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>安裝 Active Directory 憑證服務  

> [!TIP]
> 如果您想要使用 Windows PowerShell 來安裝 Active Directory 憑證服務，請參閱[install-adcscertificationauthority](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcscertificationauthority?view=win10-ps) for Cmdlet 和選擇性參數。
  
1.  以 Enterprise Admins 群組和根域的 Domain Admins 群組成員的身分登入。  
  
2.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能精靈] 隨即開啟。  
  
3.  在 [在您開始前] 中，按 [下一步]。  
  
    > [!NOTE]  
    > 如果您先前在執行 [新增角色及功能精靈] 時已經選取 [預設略過這個頁面]，就不會顯示 [新增角色及功能精靈] 的 [在您開始前] 頁面。  
  
4.  在 [選取安裝類型] 中，確定已選取 [角色型或功能型安裝]，然後按 [下一步]。  
  
5.  在 [選取目的地伺服器] 中，確定已選取 [從伺服器集區選取伺服器]。 在 [伺服器集區] 中，確定已選取本機電腦。 按一下 [下一步]。  
  
6.  在 [**選取伺服器角色**] 的 [**角色**] 中，選取 [ **Active Directory 憑證服務**]。 當系統提示您新增必要功能時，請按一下 [**新增功能**]，然後按 **[下一步]** 。  
  
7.  在 [**選取功能**] 中，按 **[下一步]** 。  
  
8.  在**Active Directory 憑證服務** 中，閱讀提供的資訊，然後按**下一步**。  
  
9. 在 [確認安裝選項] 中，按一下 [安裝]。 在安裝過程中，請勿關閉嚮導。 安裝完成時，請按一下 **[設定目的地伺服器上 Active Directory 憑證服務**]。 [AD CS 設定向導] 隨即開啟。 閱讀認證資訊，並視需要提供屬於 Enterprise Admins 群組成員之帳戶的認證。 按一下 [下一步]。  
  
10. 在 [**角色服務**] 中按一下 [**憑證授權單位**單位]，然後按 **[下一步]**  
  
11. 在 [**安裝類型**] 頁面上，確認已選取 [**企業 CA** ]，然後按 **[下一步]** 。  
  
12. 在 [**指定 CA 的類型**] 頁面上，確認已選取 [**根 ca** ]，然後按 **[下一步]** 。  
  
13. 在 [**指定私密金鑰的類型**] 頁面上，確認已選取 [**建立新的私密金鑰**]，然後按 **[下一步]** 。  
  
14. 在 [ **CA 的密碼**編譯] 頁面上，保留 CSP 的預設設定（**RSA # Microsoft 軟體金鑰儲存提供者**）和雜湊演算法（**SHA2**），並判斷您的部署的最佳金鑰字元長度。 大型的索引鍵長度提供最佳的安全性;不過，它們可能會影響伺服器效能，而且可能與繼承應用程式不相容。 建議您保留預設設定2048。 按一下 [下一步]。  
  
15. 在 [ **Ca 名稱**] 頁面上，保留 ca 的建議一般名稱，或根據您的需求變更名稱。 請確定您確定 CA 名稱與您的命名慣例和用途相容，因為您在安裝 AD CS 之後無法變更 CA 名稱。 按一下 [下一步]。  
  
16. 在 [**有效期間**] 頁面的 [**指定有效期間**] 中，輸入數位，然後選取時間值（年、月、周或日）。 建議使用5年的預設設定。 按一下 [下一步]。  
  
17. 在 [ **CA 資料庫**] 頁面的 [**指定資料庫位置**] 中，指定憑證資料庫和憑證資料庫記錄檔的資料夾位置。 如果您指定預設位置以外的位置，請確定資料夾是使用存取控制清單（Acl）來保護，以防止未經授權的使用者或電腦存取 CA 資料庫和記錄檔。 按一下 [下一步]。  
  
18. 在 [**確認**] 中，按一下 [**設定**] 以套用您的選擇，然後按一下 [**關閉**]。  
  


