---
title: compact vdisk
description: Compact vdisk 命令的參考文章，可減少動態擴充虛擬硬碟（VHD）檔案的實際大小。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7379975981c2df386b7180c814799f7129eee7da
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929051"
---
# <a name="compact-vdisk"></a>compact vdisk

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

減少動態擴充虛擬硬碟（VHD）檔案的實體大小。 這個參數很有用，因為當您新增檔案時，動態擴充 Vhd 的大小會增加，但當您刪除檔案時，不會自動縮減大小。

## <a name="syntax"></a>語法

```
compact vdisk
```

### <a name="remarks"></a>備註

- 必須選取動態擴充的 VHD，此操作才能成功。 使用 [[選取 vdisk] 命令](select-vdisk.md)來選取 VHD，並將焦點移至它。

- 您只能使用以唯讀方式卸離或附加的 compact 動態擴充 Vhd。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [附加 vdisk 命令](attach-vdisk.md)

- [detail vdisk 命令](detail-vdisk.md)

- [卸離 vdisk 命令](detach-vdisk.md)

- [展開 vdisk 命令](expand-vdisk.md)

- [Merge vdisk 命令](merge-vdisk.md)

- [選取 vdisk 命令](select-vdisk.md)

- [list 命令](list.md)
