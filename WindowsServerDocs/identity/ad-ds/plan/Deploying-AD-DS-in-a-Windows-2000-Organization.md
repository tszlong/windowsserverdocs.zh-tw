---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: 在 Windows Server 2000 組織中部署 AD DS
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f1543effdb1cd9b83c0a2016855c4c65e3b2b5c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822631"
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>在 Windows Server 2000 組織中部署 AD DS

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您的組織目前正在執行 Windows 2000 Active Directory，您可以將部分或所有網域控制站作業系統的就地升級到 Windows Server 2008，或在您的環境中引進執行 Windows Server 2008 的網域控制站，以部署 Windows Server 2008 Active Directory Domain Services （AD DS）。  
  
在您可以將執行 Windows Server 2008 的網域控制站新增到現有的 Windows 2000 Active Directory 網域之前，您必須執行**adprep**（命令列工具）。 Adprep 會擴充 AD DS 架構、更新所選物件的預設安全描述項，並依某些應用程式的需求新增目錄物件。 Adprep 適用于 Windows Server 2008 安裝磁片（\sources\adprep\adprep.exe）。 如需詳細資訊，請參閱 Adprep （[https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215)）。  
  
> [!NOTE]  
> 如果您想要將現有 Windows 2000 AD DS 網域控制站就地升級至 Windows Server 2008，您必須先將伺服器升級至 Windows Server 2003，然後將它升級至 Windows Server 2008。  
  
下圖顯示在目前執行 Windows 2000 Active Directory 的網路環境中部署 Windows Server 2008 AD DS 的步驟。  
  
![在 windows 2000 組織中部署](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> 如果您想要將網域或樹系功能等級設定為 Windows Server 2008，則您環境中的所有網域控制站都必須執行 Windows Server 2008 作業系統。  
  
將從 Windows 2000 環境就地升級的資源和帳戶網域合併為 Windows Server 2008 AD DS 部署的一部分，可能需要樹林或林內網域重建。 在樹系之間重新建立 AD DS 網域，可協助您降低組織的複雜性和相關聯的管理成本。 在樹系中重建 AD DS 網域，可協助您降低組織的系統管理負擔，方法是減少複寫流量、減少所需的使用者和群組管理，以及簡化群組原則的管理。 如需詳細資訊，請參閱 ADMT 3.1 版遷移指南（[https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678)）。  
  
如需您可以用來在目前執行 Windows 2000 Active Directory 的組織中規劃和部署 AD DS 的詳細工作清單，請參閱[檢查清單：在 Windows 2000 組織中部署 AD DS](https://technet.microsoft.com/library/cc732737.aspx)。  
  


