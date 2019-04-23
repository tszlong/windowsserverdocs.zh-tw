---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: 將您的需求和 AD DS 部署策略對應
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a61e5e6d429acd92e48a353bc829cb620dd66054
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881829"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>將您的需求和 AD DS 部署策略對應

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您完成檢閱，並找出 Active Directory 網域服務 (AD DS) 設計和部署需求，而且您判斷哪些元件與您特定的部署相關之後，您可以將這些需求對應至特定的 AD DS 部署策略。  
  
使用下表來判斷哪一種 AD DS 部署策略會對應至 AD DS 設計與部署的適當組合為貴組織的需求。 （「 是 」 表示特定的需求是必要的部署策略;「 否 」 表示特定的需求並不需要您的部署策略。）  
  
此資料表只會參考三個主要的 AD DS 部署策略本指南中所述：  
  
-   [新的組織中部署 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Windows Server 2003 組織中部署 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Windows 2000 組織中部署 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
不過，您可以建立混合式或自訂的 AD DS 部署策略，以符合您組織的需求使用 AD DS 設計與部署需求的任何組合。  
  
|AD DS 設計與部署需求|在新組織中部署 AD DS|在 Windows Server 2003 組織中部署 AD DS|在 Windows Server 2000 組織中部署 AD DS|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[適用於 Windows Server 2008 AD DS 設計的邏輯結構](https://technet.microsoft.com/library/cc770806.aspx)|是|是|是|  
|[設計適用於 Windows Server 2008 AD DS 的站台拓撲](Designing-the-Site-Topology.md)|是|是|是|  
|規劃網域控制站容量|是|是|是|  
|[部署 Windows Server 2008 樹系根網域](https://technet.microsoft.com/library/cc731174.aspx)|是|否|否|  
|[部署 Windows Server 2008 地區網域](https://technet.microsoft.com/library/cc755118.aspx)|是|是|是|  
|[啟用適用於 AD DS 的進階的功能](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|是|可以，但在環境中的所有網域控制站必須執行 Windows Server 2008 網域或樹系功能等級設定為 Windows Server 2008。|可以，但在環境中的所有網域控制站必須執行 Windows Server 2008 網域或樹系功能等級設定為 Windows Server 2008。|  
|[升級至 Windows Server 2008 和 Windows Server 2008 R2 的 AD DS 網域的 Active Directory 網域](https://technet.microsoft.com/library/cc731188.aspx)|否|是|是|  
|[重新建構樹系之間的 AD DS 網域](https://go.microsoft.com/fwlink/?LinkId=93678)|是，如果您想要試驗的網域移轉到生產環境，另一個組織與合併和合併兩個資訊技術 (IT) 基礎結構，或合併您從 Windows 中，就地升級的資源和帳戶的網域2000 或 Windows Server 2003 環境。|是，如果您想要與另一個組織合併和合併兩個的 IT 基礎結構或合併您從 Windows 2000 或 Windows Server 2003 環境中，就地升級的資源和帳戶的網域。|是，如果您想要與另一個組織合併和合併兩個的 IT 基礎結構或合併您從 Windows 2000 或 Windows Server 2003 環境中，就地升級的資源和帳戶的網域。|  
|[重新建構樹系內的 AD DS 網域](https://go.microsoft.com/fwlink/?LinkId=82740))|否|是，如果您需要減少網域的數目，減少複寫流量，以及必要的使用者和群組管理的數量，也可簡化管理的群組原則。|是，如果您需要減少網域的數目，減少複寫流量，以及必要的使用者和群組管理的數量，也可簡化管理的群組原則。|  
  


