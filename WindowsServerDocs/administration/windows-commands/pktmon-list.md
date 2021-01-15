---
title: pktmon 清單
description: Pktmon list 命令的參考文章。
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: 6f03eccb554c1a63f4a798b9896709e6ba69be8c
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241076"
---
# <a name="pktmon-list"></a>pktmon 清單

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

列出所有作用中的元件，可讓您檢查網路堆疊版面配置。 此命令會顯示網路元件 (驅動程式) 由介面卡系結排列。

## <a name="syntax"></a>語法

```
pktmon [comp] list
```

### <a name="parameters"></a>參數

| **參數** | **說明** |
| ------------- | --------------- |
| **-i，--顯示-隱藏** | 顯示預設會隱藏的元件。 |
| **--json** | 以 JSON 格式輸出清單。 |

## <a name="additional-references"></a>其他參考資料

- [Pktmon](pktmon.md)
- [Pktmon 複合](pktmon-comp.md)
- [Pktmon 計數器](pktmon-counters.md)
- [Pktmon 篩選](pktmon-filter.md)
- [Pktmon 篩選準則新增](pktmon-filter-add.md)
- [Pktmon 格式](pktmon-format.md)
- [Pktmon pcapng](pktmon-pcapng.md)
- [Pktmon 重設](pktmon-reset.md)
- [Pktmon 開始](pktmon-start.md)
- [Pktmon unload](pktmon-unload.md)
- [封包監視器總覽](/windows-server/networking/technologies/pktmon/pktmon)
