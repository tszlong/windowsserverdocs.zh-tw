---
title: compact vdisk
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7efd40fa4b822636eda9f4082b5f561b452d3846
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379297"
---
# <a name="compact-vdisk"></a>compact vdisk

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

減少動態擴充虛擬硬碟（VHD）檔案的實體大小。 這個參數很有用，因為當您新增檔案時，動態擴充 Vhd 的大小會增加，但當您刪除檔案時，不會自動縮減大小。
> [!NOTE]
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。
> ## <a name="syntax"></a>語法
> ```
> compact vdisk
> ```
> ## <a name="remarks"></a>備註
> - 必須選取動態擴充的 VHD，此操作才能成功。 使用 [**選取 vdisk** ] 命令來選取 VHD，並將焦點移至它。
> - 您只能以唯讀方式壓縮已卸離或附加的 Vhd。
>   ## <a name="BKMK_Examples"></a>典型
>   若要壓縮動態擴充的 VHD，請輸入：
>   ```
>   compact vdisk
>   ```
>   ## <a name="additional-references"></a>其他參考
> - [命令列語法關鍵](command-line-syntax-key.md)
> - [附加 vdisk](attach-vdisk.md)

-   [詳細資料 vdisk](detail-vdisk.md)
-   [卸離 vdisk](detach-vdisk.md)
-   [展開 vdisk](expand-vdisk.md)
-   [合併 vdisk](merge-vdisk.md)
-   [選取 vdisk](select-vdisk.md)
-   [list_1](list_1.md)
