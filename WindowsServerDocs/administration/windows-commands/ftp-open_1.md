---
title: ftp 開啟
description: Ftp open 命令的參考主題，它會連接到指定的 ftp 伺服器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63e428164ece405a6a83041edd46ffe332b13c3a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820388"
---
# <a name="ftp-open"></a>ftp 開啟

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接到指定的 ftp 伺服器。

## <a name="syntax"></a>語法

```
open <computer> [<port>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<computer>` | 指定您嘗試連接的遠端電腦。 您可以使用 IP 位址或電腦名稱稱（在此情況下，DNS 伺服器或主機檔案必須可以使用）。 |
| `[<port>]` | 指定要用來連接到 ftp 伺服器的 TCP 通訊埠編號。 預設會使用 TCP 埠21。 |

### <a name="examples"></a>範例

若要連接到位於*ftp.microsoft.com*的 ftp 伺服器，請輸入：

```
open ftp.microsoft.com
```

若要在接聽 TCP 通訊埠*755*的*ftp.microsoft.com*連接到 ftp 伺服器，請輸入：

```
open ftp.microsoft.com 755
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
