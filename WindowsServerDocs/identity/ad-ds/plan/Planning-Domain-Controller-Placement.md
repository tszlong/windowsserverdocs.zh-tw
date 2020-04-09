---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: 規劃設置網域控制站
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 784e26a1762e8950b8f658ea32968a0d170df13d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822181"
---
# <a name="planning-domain-controller-placement"></a>規劃設置網域控制站

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在您收集將用來設計網站拓撲的所有網路資訊之後，請規劃要放置網域控制站的位置，包括樹系根網域控制站、區域網域控制站、操作主機角色持有者和通用類別目錄伺服器。  
  
在 Windows Server 2008 中，您也可以利用唯讀網域控制站（Rodc）。 RODC 是新的網域控制站類型，主控 Active Directory 資料庫的唯讀磁碟分割。 除了帳戶密碼以外，RODC 會保存可寫入網域控制站所保有的所有 Active Directory 物件和屬性。 不過，您無法對儲存於 RODC 上的資料庫進行變更。 變更必須在可寫入的網域控制站上進行，然後複寫回 RODC。  
  
RODC 的設計主要是部署在遠端或分公司環境中，這通常會有相對較少的使用者、實體安全性不佳、中樞網站的網路頻寬相對較差，以及對資訊技術（IT）的知識有所限制的人員。 部署 Rodc 可提升安全性，並更有效率地存取網路資源。 如需 RODC 功能的詳細資訊，請參閱 AD DS：唯讀網域控制站（[https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)）。 如需有關如何部署 RODC 的詳細資訊，請參閱唯讀網域控制站的逐步指南（[https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)）。  
  
> [!NOTE]  
> 本指南不會說明如何判斷適當的網域控制站數目，以及每個網站中所代表每個網域的網域控制站硬體需求。  
  
## <a name="in-this-section"></a>本節內容  
  
-   [規劃設置樹系根網域控制站](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [規劃區域網域控制站放置](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [規劃設置通用類別目錄伺服器](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [規劃設置操作主機角色](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


