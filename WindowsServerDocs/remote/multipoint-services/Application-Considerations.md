---
title: 應用程式考量
description: MultiPoint 服務上應用程式的相容性資訊
ms.topic: article
ms.assetid: 445e6184-4e1e-4f10-ad3c-042f2a6c2f5f
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 6657e8d9f7c206cb5532eda3491af98f161ccb7a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944458"
---
# <a name="application-considerations"></a>應用程式考量

## <a name="application-compatibility"></a>應用程式相容性

您想要在 MultiPoint 服務系統上執行的任何應用程式都必須滿足下列需求：

- 它應該安裝並在 Windows Server 2016 上執行
- 它必須是會話感知，如此一來，每個使用者都可以在 MultiPoint 系統中執行應用程式的實例。

如果應用程式確實指定這項需求，建議您嘗試安裝應用程式，並在遠端桌面會話中使用它。

## <a name="addressing-application-compatibility-problems"></a>解決應用程式相容性問題
MultiPoint 服務可讓您將工作站與在同一部主機電腦上執行的完整 Windows 10 Enterprise edition 實例建立關聯。 對於不會針對多個使用者執行多個實例，或不會安裝在64位作業系統上的重要應用程式，這可能是解決方案。 以這種方式部署桌面需要使用 [MultiPoint 管理員] 中的 [虛擬桌面] 索引標籤來：

-   啟用虛擬桌面
-   建立桌面範本
-   使用問題應用程式自訂範本
-   將工作站與自訂範本建立關聯

每部工作站會從相同的範本開始，因此每次電腦啟動時，都會清除任何變更。

>[!NOTE]
>請務必確認您想要在 MultiPoint 上執行之應用程式的授權需求。 雖然您要安裝一個複製應用程式，但可能需要每個使用者的授權。

