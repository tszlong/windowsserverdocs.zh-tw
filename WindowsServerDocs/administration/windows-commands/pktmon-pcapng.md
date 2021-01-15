---
title: pktmon pcapng
description: Pktmon pcapng 命令的參考文章。
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: 7b906efd5548b6aa22168294668a1bde6da3c3c8
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241056"
---
# <a name="pktmon-pcapng"></a>pktmon pcapng

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

Pktmon pcapng 會將記錄檔轉換成 pcapng 格式。 預設不會包含捨棄的封包。

## <a name="syntax"></a>語法

```
pktmon pcapng log.etl [-o log.pcapng]
```

### <a name="parameters"></a>參數

| **參數** | **說明** |
| ------------- | --------------- |
| **-o，--out** | 格式化 pcapng 檔案的名稱。 |
| **-d、--drop** | 只轉換捨棄的封包。 |
| **-c、--元件識別碼** | 依特定元件識別碼篩選封包。 |

## <a name="example"></a>範例

下列範例只會將已卸載的封包從記錄檔 *PktMon* 中的網路介面卡轉換成 pcapng 格式：

```powershell
C:\Test> pktmon pcapng C:\tmp\PktMon.etl -d -c nics
```

## <a name="additional-references"></a>其他參考資料

- [Pktmon](pktmon.md)
- [Pktmon 複合](pktmon-comp.md)
- [Pktmon 計數器](pktmon-counters.md)
- [Pktmon 篩選](pktmon-filter.md)
- [Pktmon 篩選準則新增](pktmon-filter-add.md)
- [Pktmon 格式](pktmon-format.md)
- [Pktmon 清單](pktmon-list.md)
- [Pktmon unload](pktmon-unload.md)
- [Pktmon 重設](pktmon-reset.md)
- [Pktmon 開始](pktmon-start.md)
- [封包監視器總覽](/windows-server/networking/technologies/pktmon/pktmon)
