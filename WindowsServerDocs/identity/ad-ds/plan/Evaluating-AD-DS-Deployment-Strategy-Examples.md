---
description: 深入瞭解：評估 AD DS 部署策略範例
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: 評估 AD DS 部署策略範例
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 5612bc24e3f0662103aae1d831578c7417cc578e
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711793"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>評估 AD DS 部署策略範例

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

請考慮下列虛構公司 Contoso 製藥的範例，其會在其環境中部署 Active Directory Domain Services (AD DS) 。 Contoso 環境包含四個網域。 樹系功能等級為 Windows Server 2003。 下圖顯示 Contoso 組織目前的網域結構。

![顯示 Contoso 組織目前網域結構的圖例。](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)

在檢查其現有的環境並識別其部署目標之後，Contoso 會建立下列 AD DS 部署策略：

-   將 Windows Server 2003 網域升級至 Windows Server 2008 網域。

-   將網域和樹系功能等級提高至 Windows Server 2008，以啟用 advanced AD DS 功能。

-   在樹系中重建 africa.concorp.contoso.com 網域，以將該網域與 emea. concorp. con 網域合併。

將樹系功能等級提高至 Windows Server 2008 可讓 Contoso 充分利用新的 AD DS 功能。 重建樹系內的網域（如下圖所示），可減少管理網域所需的管理量。

![AD DS 部署策略](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)



