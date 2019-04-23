---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: 安裝 AD FS 角色服務
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9851134d1ad73092ee44c34c99bc2d873d20ca07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831169"
---
# <a name="install-the-ad-fs-role-service"></a>安裝 AD FS 角色服務

>適用於：Windows Server 2016, Windows Server 2012 R2

您可以使用下列程序在現有的同盟伺服器陣列中執行 Windows Server 2012 R2 同盟伺服器陣列中第一個同盟伺服器或同盟伺服器的電腦上安裝 AD FS 角色服務。  
  
中的成員資格**系統管理員**，或同等權限，在本機電腦上完成此程序的最低需求。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>若要安裝 AD FS 伺服器角色，透過新增角色及功能精靈  
  
1.  開啟伺服器管理員。 若要開啟 [伺服器管理員]，按一下**伺服器管理員**上**開始**畫面上，或**伺服器管理員**桌面上工作列中。 在 [儀表板] 頁面上 [歡迎使用] 磚的 [快速入門] 索引標籤中，按一下 [新增角色及功能]。 或者，您可以按一下 [管理] 功能表中的 [新增角色及功能]。  
  
2.  在 [在您開始前]  頁面上，按一下 [下一步] 。  
  
3.  在上**選取安裝類型**頁面上，按一下**角色\-型或功能型\-基礎安裝**，然後按一下**下一步**。  
  
4.  在 [選取目的地伺服器] 頁面上，按一下 [從伺服器集區選取伺服器]，確認已選取目標電腦，然後按一下 [下一步]。  
  
5.  在 [選取伺服器角色]  頁面上，按一下 [Active Directory Federation Services] ，然後按一下 [下一步] 。  
  
6.  在 [選取功能] 頁面上，按 [下一步]。 所需的必要條件是您預先選取。 您沒有選取任何其他功能。  
  
7.  在 [ **Active Directory Federation Service \(AD FS\)** 頁面上，按一下**下一步]**。  
  
8.  在確認資訊之後**確認安裝選項**頁面上，按一下**安裝**。  
  
9. 在 [安裝進度] 頁面上，確認每個項目都已正確安裝，然後按一下 [關閉]。  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>若要安裝 AD FS 伺服器角色，透過 Windows PowerShell  
  
1.  在上您想要設定為同盟伺服器的電腦，開啟 Windows PowerShell 命令視窗，然後執行下列命令： `Install-windowsfeature adfs-federation –IncludeManagementTools`。  
  
## <a name="see-also"></a>另請參閱 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

