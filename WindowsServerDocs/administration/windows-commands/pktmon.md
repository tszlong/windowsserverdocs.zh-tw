---
title: pktmon
description: 適用于 Windows 的 pktmon 網路診斷工具（可用於封包捕獲、封包卸載偵測、封包篩選和計數）的參考文章。
ms.topic: reference
author: khdownie
ms.author: v-kedow
ms.date: 1/14/2021
ms.openlocfilehash: 55b8ad90ca49349482e5e4b9ccb2ad11132a044d
ms.sourcegitcommit: 5dd48d0da9400d7e8a6ae40b4c977d1703c4e669
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241057"
---
# <a name="pktmon"></a>pktmon

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

封包監視器 (Pktmon) 是適用于 Windows 的現成、跨元件網路診斷工具。 它可用於封包捕獲、捨棄偵測、篩選和計算。 Pktmon 在虛擬化案例（例如容器網路和 SDN）中特別有用，因為它可在網路堆疊中提供可見度。

## <a name="syntax"></a>Syntax

```
pktmon { filter | comp | reset | counters | format | list | start | stop | pcapng | unload | help } [options]
```

### <a name="commands"></a>命令

| **命令** | **描述** |
| --------- | ----------- |
| [pktmon 篩選](pktmon-filter.md) | 管理封包篩選器。 |
| [pktmon 複合](pktmon-comp.md) | 管理已註冊的元件。 |
| [pktmon 重設](pktmon-reset.md) | 將計數器重設為零。 |
| [pktmon 計數器](pktmon-counters.md) | 查詢封包計數器。 |
| [pktmon 格式](pktmon-format.md) | 將記錄檔轉換成文字。 |
| [pktmon 清單](pktmon-list.md) | 列出所有作用中的元件。 |
| [pktmon 開始](pktmon-start.md) | 啟動封包監視。 |
| pktmon 停止 | 停止封包監視。 |
| [pktmon pcapng](pktmon-pcapng.md) | 將記錄檔轉換為 pcapng 格式。 |
| [pktmon unload](pktmon-unload.md) | 卸載 pktmon 驅動程式。 |
| pktmon 說明 | 顯示子命令的簡短摘要。 |

## <a name="additional-references"></a>其他參考資料

- [封包監視器總覽](/windows-server/networking/technologies/pktmon/pktmon)
- [Microsoft 網路監視器 (Netmon) 的 Pktmon 支援 ](/windows-server/networking/technologies/pktmon/pktmon-netmon-support)
