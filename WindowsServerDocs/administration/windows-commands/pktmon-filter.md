---
title: pktmon 篩選
description: Pktmon 篩選命令的參考文章。
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: 80f3978105b0c73602c8ab04d78c48d80c88c444
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241079"
---
# <a name="pktmon-filter"></a>pktmon 篩選

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

Pktmon 篩選器可讓您列出、新增或移除封包篩選器。

## <a name="syntax"></a>語法

```
pktmon filter { list | add | remove } [OPTIONS | help]
```

### <a name="parameters"></a>參數

| **參數** | **說明** |
| ------------- | --------------- |
| **pktmon 篩選清單** | 顯示作用中的封包篩選器。 |
| **pktmon 篩選準則新增** |  新增篩選器來控制要報告的封包。 |
| **pktmon 篩選準則移除** | 移除所有的封包篩選器。 |

## <a name="additional-references"></a>其他參考資料

- [Pktmon](pktmon.md)
- [Pktmon 複合](pktmon-comp.md)
- [Pktmon 計數器](pktmon-counters.md)
- [Pktmon 篩選準則新增](pktmon-filter-add.md)
- [Pktmon 格式](pktmon-format.md)
- [Pktmon 清單](pktmon-list.md)
- [Pktmon pcapng](pktmon-pcapng.md)
- [Pktmon 重設](pktmon-reset.md)
- [Pktmon 開始](pktmon-start.md)
- [Pktmon unload](pktmon-unload.md)
- [封包監視器總覽](/windows-server/networking/technologies/pktmon/pktmon)
