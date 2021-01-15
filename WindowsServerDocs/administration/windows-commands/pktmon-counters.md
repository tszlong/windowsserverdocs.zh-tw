---
title: pktmon 計數器
description: Pktmon 計數器命令的參考文章。
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: df61499d96ec8b9eee6ee2939860e95431478b52
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241083"
---
# <a name="pktmon-counters"></a>pktmon 計數器

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

Pktmon 計數器可讓您查詢和顯示目前每個元件的計數器，以確認預期的流量是否存在，並取得在機器中流動流量的高階觀點。

## <a name="syntax"></a>語法

```
pktmon [comp] counters [-t { all | drop | flow }] [-z] [--json]
```

### <a name="parameters"></a>參數

| **參數** | **說明** |
| ------------- | --------------- |
| **-t，--counter 類型** | 選取要顯示的計數器類型。 支援的值為 (預設) 、只卸載或流程的所有計數器。 |
| **-z、--顯示-零** | 顯示兩個方向為零的計數器。 |
| **-i，--顯示-隱藏** | 顯示預設會隱藏的元件。 |
| **--json** | 以 JSON 格式輸出計數器。 |

## <a name="additional-references"></a>其他參考資料

- [Pktmon](pktmon.md)
- [Pktmon 複合](pktmon-comp.md)
- [Pktmon 篩選](pktmon-filter.md)
- [Pktmon 篩選準則新增](pktmon-filter-add.md)
- [Pktmon 格式](pktmon-format.md)
- [Pktmon 清單](pktmon-list.md)
- [Pktmon pcapng](pktmon-pcapng.md)
- [Pktmon 重設](pktmon-reset.md)
- [Pktmon 開始](pktmon-start.md)
- [Pktmon unload](pktmon-unload.md)
- [封包監視器總覽](/windows-server/networking/technologies/pktmon/pktmon)
