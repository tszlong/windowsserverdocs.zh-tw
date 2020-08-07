---
title: bdehdcfg driveinfo
description: Bdehdcfg system.io.driveinfo 命令的參考文章，其中顯示磁碟機號、總大小、可用空間上限，以及磁碟分割特性。
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4836105bf3141cef036aa4f2e2630ea266e150d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895136"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg： system.io.driveinfo

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示磁碟機號、總大小、可用空間上限，以及磁碟分割特性。 只會列出有效的磁碟分割。 如果已有四個主要或擴充分割區，則不會列出未配置的空間。

>[!NOTE]
> 此命令僅供參考，而且不會變更磁片磁碟機。

## <a name="syntax"></a>語法

```
bdehdcfg -driveinfo <drive_letter>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| <drive_letter> | 指定後面接著冒號的磁碟機號。 |

## <a name="example"></a>範例

若要顯示 C：磁片磁碟機的磁片磁碟機資訊：

```
bdehdcfg  driveinfo C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
