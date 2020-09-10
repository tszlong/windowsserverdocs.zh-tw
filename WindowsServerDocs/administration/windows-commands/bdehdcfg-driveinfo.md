---
title: bdehdcfg driveinfo
description: Bdehdcfg getdrives 命令的參考文章，其中顯示磁碟機號、大小總計、可用空間上限和磁碟分割特性。
ms.topic: reference
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fb474e40e92979f5f2cf73d90a553bbf785c0312
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632965"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg： getdrives

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示磁碟機號、總大小、可用空間上限和磁碟分割特性。 只會列出有效的資料分割。 如果有四個主要或延伸磁碟分割，則不會列出未配置的空間。

>[!NOTE]
> 此命令僅供參考，且不會變更磁片磁碟機。

## <a name="syntax"></a>語法

```
bdehdcfg -driveinfo <drive_letter>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| <drive_letter> | 指定磁碟機號，後面接著冒號。 |

## <a name="example"></a>範例

若要顯示 C：磁片磁碟機的磁片磁碟機資訊：

```
bdehdcfg  driveinfo C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
