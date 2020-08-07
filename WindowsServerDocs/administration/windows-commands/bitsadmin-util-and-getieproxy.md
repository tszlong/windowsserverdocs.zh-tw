---
title: bitsadmin util and getieproxy
description: Bitsadmin util 和 getieproxy 命令的參考文章，可抓取指定服務帳戶的 proxy 使用方式。
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78d24395a22d41369127b115971597598b69ec05
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880927"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util and getieproxy

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取指定服務帳戶的 proxy 使用方式。 此命令會顯示每個 proxy 使用的值，而不只是您為服務帳戶指定的 proxy 使用方式。 如需設定特定服務帳戶之 proxy 使用方式的詳細資訊，請參閱[bitsadmin util 和 setieproxy](bitsadmin-util-and-setieproxy.md)命令。

## <a name="syntax"></a>語法

```
bitsadmin /util /getieproxy <account> [/conn <connectionname>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
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
