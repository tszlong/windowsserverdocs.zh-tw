---
ms.assetid: 7530cafe-98d7-46c9-95d9-e49d39caa021
title: 在 Windows Server 2000 組織中部署 AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5015c0ca2c01fca966927af4207a7e3501d9cd39
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854279"
---
# <a name="deploying-ad-ds-in-a-windows-2000-organization"></a>在 Windows Server 2000 組織中部署 AD DS

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

如果您的組織目前正在執行 Windows 2000 Active Directory，您可以部署 Windows Server 2008 Active Directory 網域服務 (AD DS) 所執行的部分或全部的 Windows 網域控制站的作業系統就地升級Server 2008 或藉由引進到您的環境中執行 Windows Server 2008 網域控制站。  
  
您可以新增至現有的 Windows 2000 Active Directory 網域中執行 Windows Server 2008 的網域控制站之前，您必須執行**adprep**，命令列工具。 Adprep 延伸 AD DS 結構描述、 更新選取的物件，預設安全性描述元和所需的某些應用程式中加入新的目錄物件。 Adprep 可用於 Windows Server 2008 安裝光碟 (\sources\adprep\adprep.exe)。 如需詳細資訊，請參閱 Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215))。  
  
> [!NOTE]  
> 如果您想要執行的現有 Windows 2000 AD DS 網域控制站到 Windows Server 2008 就地升級，您必須先將伺服器升級至 Windows Server 2003，並再將它升級到 Windows Server 2008。  
  
下圖顯示部署 Windows Server 2008 AD DS，目前正在執行 Windows 2000 Active Directory 的網路環境中的步驟。  
  
![在 windows 2000 組織中部署](media/Deploying-AD-DS-in-a-Windows-2000-Organization/ee51218a-a858-49d9-8b99-9986679191c1.gif)  
  
> [!NOTE]  
> 如果您想要的網域或樹系功能等級設定至 Windows Server 2008，您的環境中的所有網域控制站必須都執行 Windows Server 2008 作業系統。  
  
藉由合併會從 Windows 2000 環境中，就地升級為 Windows Server 2008 AD DS 的一部分的資源和帳戶的網域樹系或樹系內網域重建可能需要部署。 重新建構樹系之間的 AD DS 網域，可協助您降低您的組織與相關聯的系統管理成本的複雜性。 重新建構樹系中的 AD DS 網域可協助您減少為貴組織的系統管理額外負荷降低複寫流量，降低使用者和群組管理所需的數量以及簡化的管理群組原則。 如需詳細資訊，請參閱 ADMT v3.1 移轉指南 ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678))。  
  
如需您可以使用計劃和部署組織目前正在執行 Windows 2000 Active Directory 中的 AD DS 的詳細工作的清單，請參閱[檢查清單：Windows 2000 組織中部署 AD DS](https://technet.microsoft.com/library/cc732737.aspx)。  
  


