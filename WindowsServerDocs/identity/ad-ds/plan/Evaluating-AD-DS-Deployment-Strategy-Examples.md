---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: 評估 AD DS 部署策略範例
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 228aec00fab5e1ca4e136a0f75cb5e2294d66700
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822491"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>評估 AD DS 部署策略範例

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

請考慮下列虛構公司 Contoso 製藥的範例，它會在其環境中部署 Active Directory Domain Services （AD DS）。 Contoso 環境包含四個網域。 樹系功能等級為 Windows Server 2003。 下圖顯示 Contoso 組織目前的網域結構。  
  
![AD DS 部署策略](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
在審查其現有的環境並識別其部署目標之後，Contoso 已建立下列 AD DS 部署策略：  
  
-   將 Windows Server 2003 網域升級至 Windows Server 2008 網域。  
  
-   將網域和樹系功能等級提高至 Windows Server 2008，以啟用 advanced AD DS 功能。  
  
-   重建樹系中的 africa.concorp.contoso.com 網域，以合併該網域與 emea. concorp 的網域。  
  
將樹系功能等級提高至 Windows Server 2008 可讓 Contoso 充分利用新的 AD DS 功能。 重建樹系中的網域（如下圖所示），將可減少管理網域所需的管理量。  
  
![AD DS 部署策略](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


