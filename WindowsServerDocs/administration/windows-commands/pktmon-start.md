---
title: pktmon 開始
description: Pktmon start 命令的參考文章。
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: 92e8048958dbba1e4d8b71addbc010f5ba8bc31c
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241061"
---
# <a name="pktmon-start"></a>pktmon 開始

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

開始封包監視。

## <a name="syntax"></a>語法

```
pktmon start [-c { all | nics | [ids...] }] [-d] [--etw [-p size] [-k keywords]]
             [-f] [-s] [--log-mode {circular | multi-file | real-time | memory}]
```

### <a name="parameters"></a>參數

| **參數** | **說明** |
| ------------- | --------------- |
| **-c、--元件** | 選取要監視的元件。 可以是所有元件、僅限 Nic 或元件識別碼清單。 預設值為 [全部]。 |
| **-d、--drop** | 只有報告捨棄的封包。 根據預設，也會報告成功的封包傳播。 |
| **--etw** | 啟動封包捕獲的記錄會話。 |
| **-p、--封包大小** | 要從每個封包記錄的位元組數目。 若要一律記錄整個封包，請將此設定為0。 預設值為128個位元組。 |
| **-k、--關鍵字** | 十六進位位元遮罩 (也就是下列旗標的總和，) 可控制要記錄哪些事件。 預設值為0x012。 請參閱下方的關鍵字旗標。 |
| **-f、--file-name** | .etl 記錄檔。 預設值為 PktMon。 |
| **-s、--file-size** | 記錄檔大小上限（以 mb 為單位）。 預設值為 512 MB。 |
| **-l，--log 模式** | 選取記錄模式。 預設值為迴圈。 請參閱下方的記錄模式。 |

### <a name="keyword-flags"></a>關鍵字旗標

下列旗標適用于 **-k** 或 **--關鍵字** 參數 (請參閱上述) 。

| **旗標** | **描述** |
| --------- | ----------- |
| **0x001** | 內部封包監視錯誤。
| **0x002** | 元件、計數器和篩選器的相關資訊。 這項資訊會加入至記錄檔的結尾。
| **0x004** | NET_BUFFER_LIST 群組中第一個封包的來源和目的地資訊。
| **0x008** | 從 NDIS_NET_BUFFER_LIST_INFO 列舉中選取封包中繼資料。
| **0x010** | 原始封包，截斷為 [--packet size] 參數中所指定的大小。

### <a name="logging-modes"></a>記錄模式

記錄模式是使用 **-l** 或 **--log 模式** 參數所設定 (請參閱上述) 。

| **模式** | **描述** |
| -------- | --------------- |
| **迴圈** | 當達到檔案大小上限時，新的事件會覆寫最舊的事件。 |
| **多檔案** | 當達到檔案大小上限時，就會建立新的記錄檔。 記錄檔會依序編號： PktMon1、PktMon2 和 etl 等等。 |
| **即時** | 即時顯示畫面上的事件和封包。 未建立任何記錄檔。 按 **Ctrl + C** 以停止監視。 |
| **記憶** | 事件會寫入至圓形記憶體緩衝區。 緩衝區大小是在 [--file-size] 參數中指定。 緩衝區內容會在停止操作期間寫入記錄檔。 |

## <a name="examples"></a>範例

下列命令會啟動 capture 並啟用封包記錄：

```PowerShell
C:\Test> pktmon start --etw
```

下列命令會啟動捕獲、啟用封包記錄，並選取即時記錄模式：

```PowerShell
C:\Test> pktmon start --etw -l real-time
```

下列命令將會捕捉所有網路介面卡的封包計數器，而不會記錄封包：

```PowerShell
C:\Test> pktmon start -c nics
```

下列命令只會捕捉經過元件4和5的已捨棄封包，並加以記錄：

```PowerShell
C:\Test> pktmon start --etw -c 4,5 -d
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
- [Pktmon unload](pktmon-unload.md)
- [封包監視器總覽](/windows-server/networking/technologies/pktmon/pktmon)
