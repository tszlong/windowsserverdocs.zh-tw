---
title: bdehdcfg system.io.driveinfo
description: '\* * Bdehdcfg： system.io.driveinfo * * 的 Windows 命令主題-顯示磁碟機號、總大小、可用空間上限和磁碟分割特性。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0f4541bfd71fb7639d18e6e548559ed02918815
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382266"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg： system.io.driveinfo

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示磁碟機號、總大小、可用空間上限，以及磁碟分割特性。 只會列出有效的磁碟分割。 如果已有四個主要或擴充分割區，則不會列出未配置的空間。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。
## <a name="syntax"></a>語法
```
bdehdcfg -driveinfo <DriveLetter>
```
### <a name="parameters"></a>參數

|   參數   |                  描述                  |
|---------------|-----------------------------------------------|
| <DriveLetter> | 指定後面接著冒號的磁碟機號。 |

## <a name="remarks"></a>備註
此命令僅供參考，而且不會對磁片磁碟機進行任何修改。
## <a name="BKMK_Examples"></a>實例
下列範例會顯示磁片磁碟機 C 的磁片磁碟機資訊。
```
bdehdcfg  driveinfo C:
```
## <a name="additional-references"></a>其他參考
-   [命令列語法關鍵](command-line-syntax-key.md)
-   [bdehdcfg](bdehdcfg.md)
