---
title: fsutil wim
description: Fsutil wim 命令的參考文章，其提供探索和管理 Windows 映像 (WIM) 支援檔案的功能。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: f709ec86924f24e7321e4de14d3e21615f207903
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889743"
---
# <a name="fsutil-wim"></a>fsutil wim

> 適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows 10

提供探索和管理 Windows 映像 (WIM) 支援檔案的功能。

## <a name="syntax"></a>語法

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| enumfiles | 列舉支援 WIM 的檔案。 |
| `<drive name>` | 指定磁片磁碟機名稱。 |
| `<data source>` | 指定資料來源。 |
| enumwims | 列舉支援的 WIM 檔案。 |
| queryfile | 查詢檔案是否受到 WIM 的支援，如果是，則會顯示 WIM 檔案的詳細資料。 |
| `<filename>` | 指定檔案名。 |
| removewim | 從備份檔案移除 WIM。 |

### <a name="examples"></a>範例

若要從資料來源0列舉磁片磁碟機 C：的檔案，請輸入：

```
fsutil wim enumfiles C: 0
```

若要列舉磁片磁碟機 C：的支援 WIM 檔案，請輸入：

```
fsutil wim enumwims C:
```

若要查看檔案是否受到 WIM 的支援，請輸入：

```
fsutil wim C:\Windows\Notepad.exe
```

若要從磁片區 C：和資料來源2的備份檔案中移除 WIM，請輸入：

```
fsutil wim removewims C: 2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
