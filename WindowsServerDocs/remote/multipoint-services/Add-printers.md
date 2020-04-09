---
title: 新增印表機
description: 為 MultiPoint 服務使用者新增印表機。
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: e1f6d3ca-8caf-4aa0-b0ea-93cdfd3f3f37
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 7be399a73ee37413e01ef714cfd02d4e8750ab79
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856081"
---
# <a name="add-printers"></a>新增印表機
使用本主題中的程式，讓 MultiPoint 服務系統上的所有使用者都可以使用本機印表機。  
  
> [!NOTE]  
> 如果您使用具有 MultiPoint 服務的網域帳戶，使用者可以從工作站使用任何網路印表機。  
  
1.  將印表機連接到 Multipoint 伺服器。  
  
2.  將印表機設定為共用印表機：  
  
    1.  以系統管理員身分登入 MultiPoint Server 電腦。  
  
    2.  從 [開始] 畫面，開啟 [控制台]。  
  
    3.  在 [控制台] 中，按一下 [**硬體**]，然後按一下 [**裝置和印表機**]。  
  
    4.  在 [**印表機和傳真**] 底下，以滑鼠右鍵按一下印表機，然後按一下 [**印表機**內容]。  
  
    5.  按一下 [**共用**] 索引標籤。  
  
    6.  按一下 [**共用此印表機**]，指定印表機的共用名稱，然後按一下 **[確定]** 。  
  
登入任何已連接到 Multipoint 服務電腦之工作站的使用者，將能夠看到並使用印表機。 