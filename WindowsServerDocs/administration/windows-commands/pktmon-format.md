---
title: pktmon 格式
description: Pktmon format 命令的參考文章。
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: f40ea62f4a15bc52a18a8e9bbccdc45745aaa93b
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241058"
---
# <a name="pktmon-format"></a>pktmon 格式

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

Pktmon 格式會將記錄檔轉換成文字格式。

## <a name="syntax"></a>語法

```
pktmon format log.etl [-o log.txt] [-b] [-v [level]] [-x] [-e] [-l [port]
```

### <a name="parameters"></a>參數

| **參數** | **說明** |
| ------------- | --------------- |
| **-o，--out** | 格式化文字檔的名稱。 |
| **-s、--僅限統計資料** | 顯示記錄檔統計資訊。 |
| **-b、--brief** | 縮寫的封包格式。 |
| **-v, --verbose** | 詳細資訊層級 [1.. 3]。 |
| **-x、--hex** | 十六進位格式。 |
| **-e，--無乙太網路** | 請勿列印 ethernet 標頭。 |
| **-l，--vxlan** | 自訂 VXLAN 埠。 |

## <a name="additional-references"></a>其他參考資料

- [Pktmon](pktmon.md)
- [Pktmon 複合](pktmon-comp.md)
- [Pktmon 計數器](pktmon-counters.md)
- [Pktmon 篩選](pktmon-filter.md)
- [Pktmon 篩選準則新增](pktmon-filter-add.md)
- [Pktmon 清單](pktmon-list.md)
- [Pktmon pcapng](pktmon-pcapng.md)
- [Pktmon 重設](pktmon-reset.md)
- [Pktmon 開始](pktmon-start.md)
- [Pktmon unload](pktmon-unload.md)
- [封包監視器總覽](/windows-server/networking/technologies/pktmon/pktmon)
