---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: "將您的需求對應到 AD DS 部署策略"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b4f625f06fede5b9dc751282cda19b68d7f9e535
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>將您的需求對應到 AD DS 部署策略

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您完成檢視，並找出的設計 Active Directory Domain Services (AD DS)，其中的相關特定部署部署需求和您判斷之後，您可以將這些需求對應到特定 AD DS 部署策略。  
  
使用下表判斷到 AD DS 設計和部署適當組合地圖的 AD DS 部署策略適用於您組織的需求。 （「 是 」 表示特定需求是需要您的部署策略。「 不 」 表示特定需求並不需要您的部署策略。）  
  
此表格指的是只三個主要 AD DS 部署策略本文中所述：  
  
-   [新的組織中部署 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [部署 Windows Server 2003 組織中的 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Windows 2000 組織中部署 AD DS](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
不過，您可以使用任何 AD DS 設計和部署需求的組合，您組織的需求建立混合式或自訂 AD DS 部署策略。  
  
|AD DS 設計和部署需求|新的組織中部署 AD DS|部署 Windows Server 2003 組織中的 AD DS|Windows 2000 組織中部署 AD DS|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Windows Server 2008 AD DS 設計的邏輯結構](https://technet.microsoft.com/library/cc770806.aspx)|[是]|[是]|[是]|  
|[Windows Server 2008 AD DS 設計網站拓撲](Designing-the-Site-Topology.md)|[是]|[是]|[是]|  
|規劃網域控制站容量|[是]|[是]|[是]|  
|[部署 Windows Server 2008 森林根網域](https://technet.microsoft.com/library/cc731174.aspx)|[是]|否]|否]|  
|[部署 Windows Server 2008 區域網域](https://technet.microsoft.com/library/cc755118.aspx)|[是]|[是]|[是]|  
|[進階的功能讓 AD ds](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|[是]|是，但環境中的所有網域控制站必須執行 Windows Server 2008 之前您設定 Windows Server 2008 的網域或森林功能層級。|是，但環境中的所有網域控制站必須執行 Windows Server 2008 之前您設定 Windows Server 2008 的網域或森林功能層級。|  
|[Active Directory 網域升級到 Windows Server 2008 和 Windows Server 2008 R2 AD DS 網域](https://technet.microsoft.com/library/cc731188.aspx)|否]|[是]|[是]|  
|[重新建構樹系之間 AD DS 網域](https://go.microsoft.com/fwlink/?LinkId=93678)|是的如果您想要試用的網域移轉到您的實際執行環境，與其他公司合併和整合兩個資訊技術 (IT) 基礎結構，或升級 Windows 2000 或 Windows Server 2003 環境從就地的資源和 account 網域彙總。|是的如果您想要與其他公司合併和整合兩個 IT 基礎架構或整合升級位置來自 Windows 2000 或 Windows Server 2003 的環境中的資源和 account 網域。|是的如果您想要與其他公司合併和整合兩個 IT 基礎架構或整合升級位置來自 Windows 2000 或 Windows Server 2003 的環境中的資源和 account 網域。|  
|[重新建構 AD DS 網域中的樹系](https://go.microsoft.com/fwlink/?LinkId=82740))|否]|是的如果您需要的網域減少，降低複寫交通和數量必要的使用者和群組管理，或簡化的群組原則管理。|是的如果您需要的網域減少，降低複寫交通和數量必要的使用者和群組管理，或簡化的群組原則管理。|  
  


