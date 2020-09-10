---
title: nfsstat
description: Nfsstat 命令的參考文章，此命令會顯示網路檔案系統 (NFS) 和遠端程序呼叫 (RPC) 呼叫的相關統計資訊。
ms.topic: reference
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d0efecc7a23904a306221064eeccbeeee8f2c598
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635800"
---
# <a name="nfsstat"></a>nfsstat

此命令列公用程式會顯示網路檔案系統 (NFS) 的統計資訊，以及 (RPC) 呼叫的遠端程序呼叫。 使用時不含參數，此命令會顯示所有統計資料，而不會重設任何資料。

## <a name="syntax"></a>語法

```
nfsstat [-c][-s][-n][-r][-z][-m]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -c | 只顯示用戶端傳送和拒絕的用戶端 NFS 和 RPC 和 NFS 呼叫。 若只要顯示 NFS 或 RPC 資訊，請將此旗標與 **-n** 或 **-r** 參數合併。 |
| -S | 只顯示伺服器傳送和拒絕的伺服器端 NFS 和 RPC 和 NFS 呼叫。 若只要顯示 NFS 或 RPC 資訊，請將此旗標與 **-n** 或 **-r** 參數合併。 |
| -M | 顯示掛接選項所設定之掛接旗標的相關資訊、系統內部的掛接旗標，以及其他掛接資訊。 |
| -n | 顯示用戶端和伺服器的 NFS 資訊。 若只要顯示 NFS 用戶端或伺服器資訊，請將此旗標與 **-c** 或 **-s** 參數結合。 |
| -r | 顯示用戶端和伺服器的 RPC 資訊。 若只要顯示 RPC 用戶端或伺服器資訊，請將此旗標與 **-c** 或 **-s** 參數結合。 |
| -Z | 重設通話統計資料。 此旗標只適用于根使用者，可以與任何其他參數結合，以在顯示統計資料之後重設特定的統計資料集。 |

### <a name="examples"></a>範例

若要顯示用戶端傳送和拒絕之 RPC 和 NFS 呼叫數目的相關資訊，請輸入：

```
nfsstat -c
```

若要顯示和列印用戶端 NFS 通話相關資訊，請輸入：

```
nfsstat -cn
```

若要顯示用戶端和伺服器的 RPC 通話相關資訊，請輸入：

```
nfsstat -r
```

若要顯示伺服器接收和拒絕之 RPC 和 NFS 呼叫數目的相關資訊，請輸入：

```
nfsstat -s
```

若要在用戶端和伺服器上將所有呼叫相關資訊重設為零，請輸入：

```
nfsstat -z
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [網路檔案系統服務命令參考資料](services-for-network-file-system-command-reference.md)

- [NFS Cmdlet 參考](/powershell/module/nfs)
