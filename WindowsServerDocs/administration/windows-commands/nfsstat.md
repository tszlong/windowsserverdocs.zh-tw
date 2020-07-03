---
title: nfsstat
description: Nfsstat 命令的參考文章，它會顯示有關網路檔案系統（NFS）和遠端程序呼叫（RPC）呼叫的統計資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 833081baa3ae9a0c2493623a7d015334087ee26d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934793"
---
# <a name="nfsstat"></a>nfsstat

一種命令列公用程式，可顯示網路檔案系統（NFS）和遠端程序呼叫（RPC）呼叫的相關統計資訊。 使用不含參數的情況下，此命令會顯示所有統計資料，而不重設任何專案。

## <a name="syntax"></a>語法

```
nfsstat [-c][-s][-n][-r][-z][-m]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| -c | 只顯示用戶端所傳送和拒絕的用戶端 NFS 和 RPC 和 NFS 呼叫。 若只要顯示 NFS 或 RPC 資訊，請將此旗標與 **-n**或 **-r**參數結合。 |
| -S | 只顯示伺服器所傳送和拒絕的伺服器端 NFS 和 RPC 和 NFS 呼叫。 若只要顯示 NFS 或 RPC 資訊，請將此旗標與 **-n**或 **-r**參數結合。 |
| -M | 顯示掛接選項所設定的掛接旗標、系統內部掛接旗標，以及其他掛接資訊的相關資訊。 |
| -n | 顯示用戶端和伺服器的 NFS 資訊。 若只要顯示 NFS 用戶端或伺服器資訊，請將此旗標與 **-c**或 **-s**參數結合。 |
| -r | 顯示用戶端和伺服器的 RPC 資訊。 若只要顯示 RPC 用戶端或伺服器資訊，請將此旗標與 **-c**或 **-s**參數結合。 |
| -Z | 重設呼叫統計資料。 此旗標僅供根使用者使用，而且可以與任何其他參數結合，以在顯示統計資料之後重設特定的統計資料集。 |

### <a name="examples"></a>範例

若要顯示用戶端所傳送和拒絕之 RPC 和 NFS 呼叫數目的相關資訊，請輸入：

```
nfsstat -c
```

若要顯示和列印用戶端 NFS 呼叫的相關資訊，請輸入：

```
nfsstat -cn
```

若要顯示用戶端和伺服器的 RPC 呼叫相關資訊，請輸入：

```
nfsstat -r
```

若要顯示伺服器所接收和拒絕的 RPC 和 NFS 呼叫數目的相關資訊，請輸入：

```
nfsstat -s
```

若要將用戶端和伺服器上所有呼叫相關的資訊重設為零，請輸入：

```
nfsstat -z
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [網路檔案系統服務命令參考資料](services-for-network-file-system-command-reference.md)

- [NFS Cmdlet 參考](https://docs.microsoft.com/powershell/module/nfs)
