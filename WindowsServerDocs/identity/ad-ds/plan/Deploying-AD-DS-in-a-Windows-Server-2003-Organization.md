---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: 在 Windows Server 2003 組織中部署 AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7426ba8fa3ea5077510655ea4877d0e0b69226ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408902"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>在 Windows Server 2003 組織中部署 AD DS

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您的組織目前正在執行 Windows Server 2003 Active Directory，您可以將部分或所有網域控制站的作業系統就地升級至，以部署 Windows Server 2008 Active Directory Domain Services （AD DS） Windows Server 2008 或引進執行 Windows Server 2008 的網域控制站至您的環境。  
  
在您可以將執行 Windows Server 2008 的網域控制站新增到現有的 Windows Server 2003 Active Directory 網域之前，您必須執行**adprep**（命令列工具）。 Adprep 會擴充 AD DS 架構、更新所選物件的預設安全描述項，並依某些應用程式的需求新增目錄物件。 Adprep 適用于 Windows Server 2008 安裝磁片（\sources\adprep\adprep.exe）。 如需詳細資訊，請參閱 Adprep （[https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)）。  
  
下圖顯示在目前執行 Windows Server 2003 Active Directory 的網路環境中部署 Windows Server 2008 AD DS 的步驟。  
  
![在 windows 2003 組織中部署](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> 如果您想要將網域或樹系功能等級設定為 Windows Server 2008，則您環境中的所有網域控制站都必須執行 Windows Server 2008 作業系統。  
  
將從 Windows Server 2003 環境就地升級的資源網域和帳戶網域合併為 Windows Server 2008 AD DS 部署的一部分，可能需要樹林或林內網域重建。 在樹系之間重建 AD DS 網域，可協助您降低組織在 AD DS 中的呈現複雜度，並協助降低相關的管理成本。 在樹系中重建 AD DS 網域，可協助您降低組織的系統管理負擔，方法是減少複寫流量、減少所需的使用者和群組管理，以及簡化群組的管理。策略. 如需詳細資訊，請參閱 ADMT 3.1 版遷移指南（[https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)）。  
  
如需您可以用來在執行 Windows Server 2003 Active Directory 的組織中規劃和部署 AD DS 的詳細工作清單，請參閱[檢查清單：在 Windows server 2003 組織中部署 AD DS](https://technet.microsoft.com/library/cc771407.aspx)。  
  


