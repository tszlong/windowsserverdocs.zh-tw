---
title: bitsadmin util and getieproxy
description: Bitsadmin util 和 getieproxy 命令的參考文章，可抓取指定服務帳戶的 proxy 使用方式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15093341ef2dcaca6dd0f5efb4c48d4b9d1934a1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927400"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util and getieproxy

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取指定服務帳戶的 proxy 使用方式。 此命令會顯示每個 proxy 使用的值，而不只是您為服務帳戶指定的 proxy 使用方式。 如需設定特定服務帳戶之 proxy 使用方式的詳細資訊，請參閱[bitsadmin util 和 setieproxy](bitsadmin-util-and-setieproxy.md)命令。

## <a name="syntax"></a>語法

```
bitsadmin /util /getieproxy <account> [/conn <connectionname>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ---------- |
| account | 指定您想要取得其 proxy 設定的服務帳戶。 可能的值包括：<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LOCALSERVICE.</li></ul> |
| connectionname | 選擇性。 與 **/conn**參數搭配使用，以指定要使用的數據機連接。 如果您未指定 **/conn**參數，BITS 會使用 LAN 連接。 |

## <a name="examples"></a>範例

若要顯示網路服務帳戶的 proxy 使用方式：

```
bitsadmin /util /getieproxy NETWORKSERVICE
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin util 命令](bitsadmin-util.md)

- [bitsadmin 命令](bitsadmin.md)
