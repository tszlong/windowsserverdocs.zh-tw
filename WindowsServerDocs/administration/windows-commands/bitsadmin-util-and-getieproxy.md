---
title: bitsadmin util 和 getieproxy
description: 適用于**bitsadmin util 和 getieproxy**的 Windows 命令主題，可抓取指定服務帳戶的 proxy 使用方式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22b24c4f9c0941c88c70b488a82de47c7901bd8b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122479"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util 和 getieproxy

> 適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取指定服務帳戶的 proxy 使用方式。 此命令會顯示每個 proxy 使用的值，而不只是您為服務帳戶指定的 proxy 使用方式。 如需設定特定服務帳戶之 proxy 使用方式的詳細資訊，請參閱[bitsadmin util 和 setieproxy](bitsadmin-util-and-setieproxy.md)命令。

## <a name="syntax"></a>語法

```
bitsadmin /util /getieproxy <account> [/conn <connectionname>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ---------- |
| 帳戶 | 指定您想要取得其 proxy 設定的服務帳戶。 可能的值包括：<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LOCALSERVICE.</li></ul> |
| connectionname | 選擇性。 與 **/conn**參數搭配使用，以指定要使用的數據機連接。 如果您未指定 **/conn**參數，BITS 會使用 LAN 連接。 |

## <a name="examples"></a>範例

下列範例會顯示 NETWORK SERVICE 帳戶的 proxy 使用方式。

```
C:\>bitsadmin /util /getieproxy NETWORKSERVICE
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
