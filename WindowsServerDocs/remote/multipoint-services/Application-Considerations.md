---
title: 應用程式考量
description: 在 MultiPoint 服務上的應用程式的相容性資訊
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 445e6184-4e1e-4f10-ad3c-042f2a6c2f5f
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 400f87c09f1b2e897d67f94e9b7ac12ae0a1e799
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839829"
---
# <a name="application-considerations"></a>應用程式考量
  
## <a name="application-compatibility"></a>應用程式相容性

任何您想要在 MultiPoint 服務系統上執行的應用程式必須符合下列需求：
  
- 它應該安裝並執行 Windows Server 2016 上 
- 它必須是工作階段感知，因此每位使用者可以執行 MultiPoint 系統中的應用程式執行個體。
  
如果應用程式並指定這項需求建議嘗試安裝應用程式，並使用遠端桌面工作階段中。 

## <a name="addressing-application-compatibility-problems"></a>解決應用程式相容性問題  
MultiPoint 服務會提供選項來執行幾乎相同的主機電腦上的 Windows 10 企業版版本的完整執行個體相關聯站台。 重要應用程式將不會執行多個執行個體的多個使用者，或不會安裝在 64 位元作業系統上，這可以是一個方案。 這種方式部署桌面時，需要使用 MultiPoint 管理員 中的 虛擬桌面 索引標籤：  
  
-   啟用虛擬桌面  
-   建立桌面範本  
-   自訂與問題的應用程式範本  
-   站台相關聯的自訂範本  

讓任何變更就會清除每次電腦啟動的時，每個站台會開始從相同的範本。  
  
>[!NOTE] 
>請務必確認您想要在 MultiPoint 上執行的授權需求的應用程式。 雖然您要安裝一份應用程式可能需要每位使用者授權。  
  
