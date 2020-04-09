---
title: 限制使用者對伺服器的存取權
description: 瞭解如何為使用者和群組授與或拒絕 MultiPoint 服務的存取權
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 4cabd4f1-a764-4be6-bc6e-0a5f5566390c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: ddc852eea3ed17cd354cc87c79d82066f989bd5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820251"
---
# <a name="limit-users-access-to-the-multipoint-server"></a>限制使用者對 Multipoint 伺服器的存取
無論您是將 MultiPoint server 加入 Active Directory 網域，或使用本機使用者帳戶，所有使用者預設都可以存取 MultiPoint server。 在您允許使用者登入 MultiPoint 服務環境中的工作站之前，您應該限制伺服器的存取權。  
  
遠端桌面使用者群組中的任何使用者都可以登入 MultiPoint server。 根據預設，每個人都是「遠端桌面使用者」群組的成員，因此每位本機使用者和網域使用者都可以登入 MultiPoint 伺服器。 若要限制對 MultiPoint Server 的存取，請從遠端桌面使用者群組移除 Everyone 使用者群組，然後將特定的使用者或群組新增至遠端桌面使用者群組。  
  
## <a name="add-or-remove-users-or-groups-to-the-remote-desktop-users-group"></a>新增或移除遠端桌面使用者群組的使用者或群組  
  
1.  從 [**開始**] 畫面開啟 [**電腦管理**]。  
  
2.  在主控台樹的 [**本機使用者和群組**] 底下，按一下 [**群組**]。  
  
3.  按兩下 [**遠端桌面使用者**]，並遵循指示來新增或移除使用者。  
  
    -   若要限制對伺服器的一般存取，請移除 Everyone 群組。  
  
    -   若要讓您的 MultiPoint 伺服器使用者存取工作站，請將每個本機帳戶或每個網域使用者或群組帳戶新增至遠端桌面使用者群組。  