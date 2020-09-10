---
title: ftp hash
description: 'Ftp hash 命令的參考文章，此命令會切換每個已傳輸資料區塊的數位記號 ( # ) 列印。'
ms.topic: reference
ms.assetid: 76bed9b3-3efd-4a3e-831f-460626d02c44
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3fd6eabfd82c54b653458c18d6a6abe62125affc
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621681"
---
# <a name="ftp-hash"></a>ftp hash

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

針對每個傳送的資料區塊，切換數位記號 ( # ) 列印。 預設會關閉 hash 命令。 資料區塊的大小為2048個位元組。

## <a name="syntax"></a>語法

```
hash
```

### <a name="examples"></a>範例

若要切換傳送的每個資料區塊的數位正負號 ( # ) 列印，請輸入：

```
hash
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
