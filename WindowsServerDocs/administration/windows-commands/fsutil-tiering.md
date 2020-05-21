---
title: fsutil tiering
description: 適用于 [fsutil 階層處理] 命令的參考主題，可啟用儲存層功能的管理，例如設定和停用旗標和層級清單。
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 6fa8646dcdf7e836ccb45f253e0c4f8691b1ea3c
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436843"
---
# <a name="fsutil-tiering"></a>fsutil tiering

> 適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows 10

啟用儲存層功能的管理，例如設定和停用旗標和層級清單。

## <a name="syntax"></a>語法

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| clearflags | 停用磁片區的分層行為旗標。 |
| `<volume>` | 指定磁片區。 |
| /trnh | 針對具有階層式存放裝置的磁片區，將會停用熱收集。<p>僅適用于 NTFS 和 ReFS。 |
| queryflags | 查詢磁片區的分層行為旗標。 |
| regionlist | 列出磁片區的分層區域及其各自的儲存層。 |
| setflags | 啟用磁片區的分層行為旗標。 |
| tierlist | 列出與磁片區相關聯的儲存層。 |

### <a name="examples"></a>範例

若要查詢磁片區 C 上的旗標，請輸入：

```
fsutil tiering clearflags C:
```

若要在磁片區 C 上設定旗標，請輸入：

```
fsutil tiering setflags C: /trnh
```

若要清除磁片區 C 上的旗標，請輸入：

```
fsutil tiering clearflags C: /trnh
```

若要列出磁片區 C 的區域及其各自的儲存層，請輸入：

```
fsutil tiering regionlist C:
```

若要列出磁片區 C 的層級，請輸入：

```
fsutil tiering tierlist C:
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
