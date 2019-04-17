---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: "安裝 AD FS 角色服務"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9851134d1ad73092ee44c34c99bc2d873d20ca07
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-ad-fs-role-service"></a>安裝 AD FS 角色服務

>適用於：Windows Server 2016、Windows Server 2012 R2

您可以使用下列程序的電腦現有的聯盟伺服器發電廠在執行 Windows Server 2012 R2 成為聯盟伺服器陣列中的第一個聯盟伺服器或聯盟伺服器上安裝 AD FS 角色服務。  
  
資格在**系統管理員**，或相當於、在本機電腦上已完成此程序的最低需求。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>若要安裝透過新增角色與功能] 精靈 AD FS 伺服器角色  
  
1.  打開伺服器管理員。 若要打開伺服器管理員中，按一下 [**伺服器管理員**在**[開始]**畫面上，或**伺服器管理員]**在桌面工作列中。 在**快速入門**索引標籤的**歡迎使用**] 磚上**儀表板**頁面上，按一下**新增角色與功能**。 或者，您可以按一下**新增角色與功能**在**管理**功能表。  
  
2.  在**在您開始之前**頁面上，按一下 [**下**。  
  
3.  在**選取 [安裝類型**頁面上，按一下 [ **Role\ 或 Feature\ 安裝**，然後按一下 [**下一步**。  
  
4.  在**選取目的伺服器**頁面上，按一下 [**伺服器集區中選取 [伺服器**，確認的目標電腦已選取，然後按**下一步**。  
  
5.  在**選擇伺服器角色**頁面上，按一下 [ **Active Directory 同盟服務**，然後按一下 [**下一步**。  
  
6.  在**選擇功能**頁面上，按一下 [**下**。 所需的必要條件是為您預先選取。 您不必選取任何其他功能。  
  
7.  在**Active Directory 同盟服務 \(AD FS\)**頁面上，按**下**。  
  
8.  在確認此資訊後**確認安裝選項**頁面上，按**安裝**。  
  
9. 在**安裝進度**頁面，確認所有正確，安裝，然後按一下 [**關閉**。  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>若要安裝 Windows PowerShell 透過 AD FS 伺服器角色  
  
1.  在電腦上您想要設定為聯盟伺服器，開放的 Windows PowerShell 命令視窗中，，然後執行下列命令： `Install-windowsfeature adfs-federation –IncludeManagementTools`。  
  
## <a name="see-also"></a>也了 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署聯盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

