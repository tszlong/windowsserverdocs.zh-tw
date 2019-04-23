---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: 決定排程
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dee63ce0fb687b2b722ce64614c54388fc544433
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838999"
---
# <a name="determining-the-schedule"></a>決定排程

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以設定站台連結的排程來控制站台連結的可用性。 當兩個站台間複寫會周遊多個站台連結時，在所有相關的連結上的複寫排程的交集會判斷兩個站台之間的連線排程。  
  
若要規劃設定站台連結排程，請建立包含彼此直接複寫的網域控制站的站台連結之間的兩個重疊的排程。 除非您想要封鎖在尖峰時段的複寫流量，請使用這些連結上的預設 （100 個可用百分比） 的排程。 封鎖的複寫，優先順序為其他流量，但您也會增加複寫延遲。  
  
網域控制站儲存時間以 Coordinated Universal Time (UTC)。 在 站台連結物件排程的時間設定符合網站以及電腦所在的排程已設定為本地時間。 當網域控制站連絡不同站台和時區的電腦時，網域控制站上的排程會顯示根據電腦的站台的當地時間的時間設定。  
  


