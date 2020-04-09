---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: 安裝 AD FS 角色服務
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 942d34cbfcd988820a54a4f85583ce4b424a415f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855391"
---
# <a name="install-the-ad-fs-role-service"></a>安裝 AD FS 角色服務

您可以使用下列程式，在執行 Windows Server 2012 R2 的電腦上安裝 AD FS 角色服務，使其成為同盟伺服器陣列中的第一部同盟伺服器，或現有同盟伺服器陣列中的同盟伺服器。  
  
若要完成此程式，至少需要本機電腦上之**Administrators**的成員資格或同等許可權。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>透過新增角色及功能精靈安裝 AD FS 伺服器角色  
  
1.  開啟 [伺服器管理員]。 若要開啟伺服器管理員，請按一下 [**開始**] 畫面上的 [**伺服器管理員**]，或桌面上工作列中的 [**伺服器管理員**]。 在 [儀表板] 頁面中，於 [歡迎畫面] 磚的 [快速入門] 索引標籤中按一下 [新增角色及功能]。 您也可以在 [管理] 功能表中按一下 [新增角色及功能]。  
  
2.  在 [在您開始前] 頁面中按 [下一步]。  
  
3.  在 [**選取安裝類型**] 頁面上，依序按一下 [**角色\-] 或 [功能\-型安裝**]，然後按 **[下一步]** 。  
  
4.  在 [選取目的地伺服器] 頁面上，按一下 [從伺服器集區選取伺服器]，確認已選取目標電腦，然後按一下 [下一步]。  
  
5.  在 [選取伺服器角色] 頁面上，按一下 [Active Directory Federation Services]，然後按 [下一步]。  
  
6.  在 [選取功能] 頁面上，按 [下一步]。 系統會為您預先選取必要的必要條件。 您不需要選取任何其他功能。  
  
7.  在 [ **Active Directory 同盟服務 \(AD FS\)** ] 頁面上，按 **[下一步]** 。  
  
8.  確認 [確認安裝選項] 頁面中的資訊後，按一下 [安裝]。  
  
9. 在 [安裝進度] 頁面上，確認每個項目都已正確安裝，然後按一下 [關閉]。  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>透過 Windows PowerShell 安裝 AD FS 伺服器角色  
  
1.  在您要設定為同盟伺服器的電腦上，開啟 Windows PowerShell 命令視窗，然後執行下列命令： `Install-windowsfeature adfs-federation –IncludeManagementTools`。  
  
## <a name="see-also"></a>另請參閱 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

