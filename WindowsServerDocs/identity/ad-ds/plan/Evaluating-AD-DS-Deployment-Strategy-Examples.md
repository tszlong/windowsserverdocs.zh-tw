---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: 評估 AD DS 部署策略範例
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 169d0a55f9fb167390c13ac1c89f8d68427f318d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842109"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>評估 AD DS 部署策略範例

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

請考慮下列虛構公司 Contoso Pharmaceuticals 在其環境中部署 Active Directory 網域服務 (AD DS) 的範例。 Contoso 環境是由四個定義域所組成。 樹系功能等級是 Windows Server 2003。 下圖顯示目前的網域結構，在 Contoso 組織。  
  
![AD DS 部署策略](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
之後檢閱現有的環境，並識別其部署目標，Contoso 會建立下列的 AD DS 部署策略：  
  
-   升級至 Windows Server 2008 網域的 Windows Server 2003 網域。  
  
-   啟用 AD DS 的進階的功能提升到 Windows Server 2008 網域和樹系功能等級。  
  
-   重組 africa.concorp.contoso.com 網域合併該網域搭配 emea.concorp.contoso.con 網域樹系內的結構。  
  
提高樹系功能等級為 Windows Server 2008 會讓 Contoso 能夠充分利用新的 AD DS 功能。 下圖中，所示，重新建構樹系中的定義域會減少系統管理，並管理網域所需的數量。  
  
![AD DS 部署策略](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


