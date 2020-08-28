---
title: fsutil resource
description: Fsutil resource 命令的參考文章，可管理交易式 Resource Manager 及其行為。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: a39bb99e570f09b57962d8b3d64ec783611ac04b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033016"
---
# <a name="fsutil-resource"></a>fsutil resource

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

建立次要交易式 Resource Manager、啟動或停止交易式 Resource Manager，或顯示交易式 Resource Manager 的相關資訊，並修改下列行為：

- 預設交易式 Resource Manager 是否會在下一次掛接時清除其交易中繼資料。

- 指定的交易式 Resource Manager，優先于可用性的一致性。

- 指定的交易 Resource Manager 優先于一致性的可用性。

- 正在執行之交易式 Resource Manager 的特性。

## <a name="syntax"></a>語法

```
fsutil resource [create] <rmrootpathname>
fsutil resource [info] <rmrootpathname>
fsutil resource [setautoreset] {true|false} <Defaultrmrootpathname>
fsutil resource [setavailable] <rmrootpathname>
fsutil resource [setconsistent] <rmrootpathname>
fsutil resource [setlog] [growth {<containers> containers|<percent> percent} <rmrootpathname>] [maxextents <containers> <rmrootpathname>] [minextents <containers> <rmrootpathname>] [mode {full|undo} <rmrootpathname>] [rename <rmrootpathname>] [shrink <percent> <rmrootpathname>] [size <containers> <rmrootpathname>]
fsutil resource [start] <rmrootpathname> [<rmlogpathname> <tmlogpathname>
fsutil resource [stop] <rmrootpathname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 建立 | 建立次要交易式 Resource Manager。 |
| `<rmrootpathname>` | 指定交易式 Resource Manager 根目錄的完整路徑。 |
| info | 顯示指定的交易式 Resource Manager 的資訊。 |
| setautoreset | 指定預設交易式 Resource Manager 是否會在下一次掛接時清除交易中繼資料。<ul><li>**true** -指定交易 Resource Manager 預設會清除下一個掛接上的交易中繼資料。</li><li>**false** -指定交易 Resource Manager 預設不會清除下一個掛接上的交易中繼資料。 |
| `<defaultrmrootpathname>` | 指定磁片磁碟機名稱，後面接著冒號。 |
| setavailable | 指定交易式 Resource Manager 將會優先于一致性的可用性。 |
| setconsistent | 指定交易式 Resource Manager 將會優先于可用性上的一致性。 |
| setlog | 變更已在執行之交易式 Resource Manager 的特性。 |
| growth | 指定交易式 Resource Manager 記錄可成長的量。<p>您可以依照下列方式指定成長參數：<ul><li>使用下列格式的容器數目： `<containers> containers`</li><li>百分比，使用格式： `<percent> percent`</li></ul> |
| `<containers>` | 指定交易式 Resource Manager 所使用的資料物件。 |
| maxextent | 指定指定之交易式 Resource Manager 的容器數目上限。 |
| minextent | 指定所指定交易式 Resource Manager 的容器數目下限。 |
| 模式 `{full|undo}` | 指定是否要記錄所有交易 ( **完整**) 或只記錄復原的事件 (**復原**) 。 |
| 重新命名 | 變更交易式 Resource Manager 的 GUID。 |
| shrink | 指定交易式 Resource Manager 記錄檔可以自動減少的百分比。 |
| 大小 | 將交易式 Resource Manager 的大小指定為指定的 *容器*數目。 |
| start | 啟動指定的交易式 Resource Manager。 |
| stop | 停止指定的交易式 Resource Manager。 |

### <a name="examples"></a>範例

若要為 *c:\test*所指定的交易式 Resource Manager 設定記錄，使其自動成長五個容器，請輸入：

```
fsutil resource setlog growth 5 containers c:test
```

若要為 *c:\test*指定的交易式 Resource Manager 設定記錄檔，使其自動成長為兩%，請輸入：

```
fsutil resource setlog growth 2 percent c:test
```

若要指定預設的交易式 Resource Manager 將在磁片磁碟機 C 上的下一次掛接時清除交易中繼資料，請輸入：

```
fsutil resource setautoreset true c:\
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [交易式 NTFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v=ws.10))
