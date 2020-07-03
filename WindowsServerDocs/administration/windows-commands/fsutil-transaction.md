---
title: fsutil transaction
description: 適用于管理 NTFS 交易之 fsutil transaction 命令的參考文章。
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 95cd9a8f62aa9dd64d46a875a90847a65589b447
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922340"
---
# <a name="fsutil-transaction"></a>fsutil transaction

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

管理 NTFS 交易。

## <a name="syntax"></a>語法

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <filename>
fsutil transaction [list]
fsutil transaction [query] [{files | all}] <GUID>
fsutil transaction [rollback] <GUID>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 認可 | 標示成功的隱含或明確指定交易的結尾。 |
| `<GUID>` | 指定代表交易的 GUID 值。 |
| fileinfo  | 顯示指定檔案的交易資訊。 |
| `<filename>` | 指定完整路徑和檔案名。 |
| list | 顯示目前正在執行的交易清單。 |
| 查詢 | 顯示指定之交易的資訊。<ul><li>如果 `fsutil transaction query files` 指定了，就只會針對指定的交易顯示檔案資訊。</li><li>如果 `fsutil transaction query all` 指定了，就會顯示該交易的所有資訊。</li></ul> |
| 復原 | 將指定的交易回復到一開始。 |

### <a name="examples"></a>範例

若要顯示檔*c:\test.txt*的交易資訊，請輸入：

```
fsutil transaction fileinfo c:\test.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [交易式 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730726(v=ws.10))
