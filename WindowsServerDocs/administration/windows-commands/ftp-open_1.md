---
title: ftp open
description: Ftp open 命令的參考文章，此命令會連接到指定的 ftp 伺服器。
ms.topic: reference
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 24fabf61e57a0dda00837a15702dfe9ef5ee6862
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635279"
---
# <a name="ftp-open"></a>ftp open

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接到指定的 ftp 伺服器。

## <a name="syntax"></a>語法

```
open <computer> [<port>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<computer>` | 指定您嘗試連接的遠端電腦。 您可以使用 IP 位址或電腦名稱稱 (在這種情況下，DNS 伺服器或 Hosts 檔案必須可供使用) 。 |
| `[<port>]` | 指定用來連接到 ftp 伺服器的 TCP 通訊埠編號。 預設會使用 TCP 埠21。 |

### <a name="examples"></a>範例

若要連接到 *ftp.microsoft.com*上的 ftp 伺服器，請輸入：

```
open ftp.microsoft.com
```

若要在接聽 TCP 通訊埠*755*的*ftp.microsoft.com*連接到 ftp 伺服器，請輸入：

```
open ftp.microsoft.com 755
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
