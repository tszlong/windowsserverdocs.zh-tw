---
title: pktmon unload
description: Pktmon unload 命令的參考文章。
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: cc7313054445bdc72bd98cb60a7f4f6d11095553
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241068"
---
# <a name="pktmon-unload"></a>pktmon unload

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

停止 PktMon 驅動程式服務，並卸載 PktMon.sys。 有效地等於 ' sc.exe stop PktMon '。 量測 (如果作用中) 將會立即停止，而且會刪除 (計數器、篩選等 ) 的任何狀態。

## <a name="syntax"></a>Syntax

```
pktmon unload
```

## <a name="additional-references"></a>其他參考資料

- [Pktmon](pktmon.md)
- [Pktmon 複合](pktmon-comp.md)
- [Pktmon 計數器](pktmon-counters.md)
- [Pktmon 篩選](pktmon-filter.md)
- [Pktmon 篩選準則新增](pktmon-filter-add.md)
- [Pktmon 格式](pktmon-format.md)
- [Pktmon 清單](pktmon-list.md)
- [Pktmon pcapng](pktmon-pcapng.md)
- [Pktmon 重設](pktmon-reset.md)
- [Pktmon 開始](pktmon-start.md)
- [封包監視器總覽](/windows-server/networking/technologies/pktmon/pktmon)
