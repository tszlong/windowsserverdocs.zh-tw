---
title: 限制使用者存取伺服器
description: 了解如何授與或拒絕存取的使用者和群組的 MultiPoint 服務
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cabd4f1-a764-4be6-bc6e-0a5f5566390c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 1466e19152847a6c7d88f77162c50ec73a5a7d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830809"
---
# <a name="limit-users-access-to-the-multipoint-server"></a>限制使用者對 Multipoint server 的存取
是否 MultiPoint 伺服器加入 Active Directory 網域，或使用本機使用者帳戶，所有使用者會都有預設的 MultiPoint 伺服器存取權。 您允許使用者登入 MultiPoint 服務環境中的站台之前，您應該限制存取伺服器。  
  
Remote Desktop Users 群組中的任何使用者可以登入 MultiPoint 伺服器。 根據預設，使用者群組每個人都是 Remote Desktop Users 群組的成員，因此每個本機使用者和網域使用者可以登入 MultiPoint Server。 若要限制對 MultiPoint 伺服器存取，移除所有的使用者群組從 Remote Desktop Users 群組，然後將特定使用者或群組新增至 Remote Desktop Users 群組。  
  
## <a name="add-or-remove-users-or-groups-to-the-remote-desktop-users-group"></a>新增或移除使用者或群組加入 Remote Desktop Users 群組  
  
1.  從**開始**畫面上，開啟**電腦管理**。  
  
2.  在主控台樹狀目錄之下**本機使用者和群組**，按一下**群組**。  
  
3.  按兩下**Remote Desktop Users**，並遵循指示來新增或移除使用者。  
  
    -   若要限制伺服器的一般存取，請移除 Everyone 群組。  
  
    -   若要讓使用者存取您的 MultiPoint 伺服器站台，將新增至 Remote Desktop Users 群組的每個本機帳戶或每個網域使用者或群組帳戶。  