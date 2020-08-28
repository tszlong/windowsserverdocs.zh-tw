---
title: merge vdisk
description: Merge vdisk 命令的參考文章，此命令會合並差異虛擬硬碟 (VHD) 與其對應的父 VHD。
ms.topic: reference
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d57f4919fbc253149343660f7239cd3405910711
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037856"
---
# <a name="merge-vdisk"></a>merge vdisk

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將差異虛擬硬碟 (VHD) 與其對應的父 VHD 合併。 父 VHD 將會修改，以包含來自差異 VHD 的修改。 此命令會修改父 VHD。 因此，相依于父系的其他差異 Vhd 將不再有效。

> [!IMPORTANT]
> 您必須選擇並卸離 VHD，此作業才會成功。 使用 [ **選取 vdisk** ] 命令選取 VHD，並將焦點移至該 VHD。

## <a name="syntax"></a>語法

```
merge vdisk depth=<n>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 深度 =`<n>` | 指出要合併的父 VHD 檔案數目。 例如， `depth=1` 表示差異 VHD 將與差異鏈的一個層級合併。 |

### <a name="examples"></a>範例

若要合併差異 VHD 與其父 VHD，請輸入：

```
merge vdisk depth=1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [attach vdisk 命令](attach-vdisk.md)

- [compact vdisk 命令](compact-vdisk.md)

- [detail vdisk 命令](detail-vdisk.md)

- [卸離 vdisk 命令](detach-vdisk.md)

- [展開 [vdisk] 命令](expand-vdisk.md)

- [選取 vdisk 命令](select-vdisk.md)

- [list 命令](list.md)
