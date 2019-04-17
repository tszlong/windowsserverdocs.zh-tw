---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: "Windows 2000 組織中部署 AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1c46ff6aee03eee4169dbfe0cdff99febad312d2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>Windows 2000 組織中部署 AD DS

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您的組織目前執行的是 Windows 2000 Active Directory，您可以執行的部分或所有的 Windows Server 2008 網域控制站的作業系統就地升級或簡介到您的環境中執行 Windows Server 2008 網域控制站部署 Windows Server 2008 Active Directory Domain Services (AD DS)。  
  
您可以加入現有的 Windows 2000 Active Directory domain 到執行 Windows Server 2008 的網域控制站之前，您必須在執行**adprep**，此命令列工具。 Adprep 延伸 AD DS 架構、更新物件選取的預設安全性描述與所需的某些應用程式中新增新的 directory 物件。 Adprep 可 (\sources\adprep\adprep.exe) 的 Windows Server 2008 安裝磁碟上。 如需詳細資訊，請查看 Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215))。  
  
> [!NOTE]  
> 如果您想要執行的 Windows Server 2008 現有 Windows 2000 AD DS 網域控制站就地升級，您必須伺服器第一次升級到 Windows Server 2003，再升級到 Windows Server 2008。  
  
下圖顯示部署 Windows Server 2008 AD DS，目前執行的是 Windows 2000 Active Directory 網路環境中的步驟。  
  
![在 windows 2000 組織中部署](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> 如果您想要為 Windows Server 2008 的網域或森林功能層級，所有的網域控制站在您的環境中必須執行 Windows Server 2008 的作業系統。  
  
整合升級的 Windows Server 2008 AD DS 一部分從 Windows 2000 環境就地的資源和 account 網域樹系內網域重新建構可能需要部署。 樹系之間 AD DS 網域重新建構可協助您減少複雜的組織和系統管理成本。 重新建構 AD DS 網域中的樹系可協助您管理您的組織負擔減少降低複寫流量、減少的使用者和群組所需的、管理及簡化的群組原則管理。 如需詳細資訊，請查看 ADMT v3.1 移轉指南 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678))。  
  
如需詳細的工作，您可以使用計劃和部署 AD DS 目前執行的是 Windows 2000 Active Directory 組織中的清單，請查看[檢查清單︰ 部署 Windows 2000 組織 AD DS](https://technet.microsoft.com/library/cc732737.aspx)。  
  


