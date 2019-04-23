---
title: MultiPoint 服務加入網域 （選用）
Description: 提供的步驟，加入您的網域中的 MultiPoint 服務
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: af5dd1f16e011161bbcf72c21c21088721ac1243
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827039"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>MultiPoint 服務電腦加入網域 （選用）
如果您將會存取您的 MultiPoint 服務電腦透過 Active Directory 網域，您的下一個步驟是將電腦加入網域。  
  
> [!IMPORTANT]  
> 您將電腦加入網域之前，您必須確認您的時區。 如需相關指示，請參閱 <<c0> [ 日期、 時間和時區設定](Set-the-date--time--and-time-zone.md)。  
   
1.  從 [開始]  畫面，開啟 [控制台] 。 按一下 **系統及安全性**，然後按一下**系統**。  
  
2.  在 [電腦名稱、網域及工作群組設定] 底下，按一下 [變更設定] 。  
  
3.  在 **電腦名稱**索引標籤上，按一下**變更**。  
  
4.  中**電腦名稱/網域變更**對話方塊中，選取**網域**，輸入網域的名稱，然後按一下**確定**，然後依照 完成精靈中的步驟程序。  
  
5.  電腦重新啟動之後，系統管理員身分登入，並等候 MultiPoint 管理員，以開啟。  
  
> [!IMPORTANT]  
> 若要確保您的 MultiPoint 服務網域部署正常運作，您必須設定數個群組原則和更新登錄。 如需資訊，請參閱[設定網域部署的群組原則](https://technet.microsoft.com/library/dn265982.aspx)。  