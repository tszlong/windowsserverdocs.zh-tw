---
title: rpcinfo
description: Rpcinfo 命令的參考文章，此命令會列出遠端電腦上的程式。
ms.topic: reference
ms.assetid: 7c342232-a8f0-42ff-8f11-d18c4981f5ca
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 3e5865bb976f71b9fbb90e4dd77008f00056a455
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639821"
---
# <a name="rpcinfo"></a>rpcinfo

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列出遠端電腦上的程式。 **Rpcinfo**命令列公用程式會進行遠端程序呼叫， (rpc) 至 rpc 伺服器，並報告它所找到的內容。

## <a name="syntax"></a>語法

```
rpcinfo [/p [<node>]] [/b <program version>] [/t <node program> [<version>]] [/u <node program> [<version>]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /p `[<node>]` | 列出在指定的主機上，以埠對應程式註冊的所有程式。 如果您未指定 (電腦) 名稱的節點，則程式會查詢本機主機上的埠對應程式。 |
| 後退 `<program version>` | 要求所有網路節點的回應，這些節點具有使用埠對應程式註冊的指定程式和版本。 您必須同時指定程式名稱或號碼，以及版本號碼。 |
| 一起 `<node program> [\<version>]` | 使用 TCP 傳輸通訊協定來呼叫指定的程式。 您必須同時指定節點 (電腦) 名稱和程式名稱。 如果您未指定版本，程式就會呼叫所有版本。 |
| u `<node program> [\<version>]` | 使用 UDP 傳輸通訊協定來呼叫指定的程式。 您必須同時指定節點 (電腦) 名稱和程式名稱。 如果您未指定版本，程式就會呼叫所有版本。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要列出所有以埠對應程式註冊的程式，請輸入：

```
rpcinfo /p [<node>]
```

若要從具有指定程式的網路節點要求回應，請輸入：

```
rpcinfo /b <program version>
```

若要使用傳輸控制通訊協定 (TCP) 呼叫程式，請輸入：

```
rpcinfo /t <node program> [<version>]
```

使用使用者資料包協定 (UDP) 來呼叫程式：

```
rpcinfo /u <node program> [<version>]
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
