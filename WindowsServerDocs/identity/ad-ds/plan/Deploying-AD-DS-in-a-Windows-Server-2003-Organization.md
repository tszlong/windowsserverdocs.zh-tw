---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: 在 Windows Server 2003 組織中部署 AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 033973ad7a726054f6c47c7154fa54a3767beab4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816359"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>在 Windows Server 2003 組織中部署 AD DS

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

如果您的組織目前正在執行 Windows Server 2003 Active Directory，您可以部署 Windows Server 2008 Active Directory 網域服務 (AD DS) 所執行的部分或所有網域控制站的作業系統就地升級 Windows Server 2008，或藉由引進到您的環境中執行 Windows Server 2008 網域控制站。  
  
您可以新增至現有的 Windows Server 2003 Active Directory 網域中執行 Windows Server 2008 的網域控制站之前，您必須執行**adprep**，命令列工具。 Adprep 延伸 AD DS 結構描述、 更新選取的物件，預設安全性描述元和所需的某些應用程式中加入新的目錄物件。 Adprep 可用於 Windows Server 2008 安裝光碟 (\sources\adprep\adprep.exe)。 如需詳細資訊，請參閱 Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215))。  
  
下圖顯示目前正在執行 Windows Server 2003 Active Directory 的網路環境中部署 Windows Server 2008 AD DS 的步驟。  
  
![在 windows 2003 組織中部署](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> 如果您想要的網域或樹系功能等級設定至 Windows Server 2008，您的環境中的所有網域控制站必須都執行 Windows Server 2008 作業系統。  
  
彙總資源網域和帳戶的網域，因為樹系或樹系內網域重新建構，可能需要 Windows Server 2008 AD DS 部署的一部分，從 Windows Server 2003 環境的就地升級。 重新建構樹系之間的 AD DS 網域可協助您降低您的組織在 AD DS 中的表示法的複雜度，並協助降低相關聯的系統管理成本。 重新建構樹系中的 AD DS 網域可協助您減少為貴組織的系統管理額外負荷降低複寫流量，減少的使用者和群組管理，為必要項，並簡化群組的管理原則。 如需詳細資訊，請參閱 ADMT v3.1 移轉指南 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678))。  
  
如需您可以使用計劃和部署執行 Windows Server 2003 Active Directory 的組織中的 AD DS 的詳細工作的清單，請參閱[檢查清單：Windows Server 2003 組織中部署 AD DS](https://technet.microsoft.com/library/cc771407.aspx)。  
  


