---
title: 合併 vdisk
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7023f2a6669ea6f6801e25cbfc87c950ab95a3bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373728"
---
# <a name="merge-vdisk"></a>合併 vdisk

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

合併差異虛擬硬碟（VHD）與其對應的父 VHD。 將修改父系 VHD 以包含差異 VHD 的修改。
> [!NOTE]
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。
> ## <a name="syntax"></a>語法
> ```
> merge vdisk depth=<n>
> ```
> ### <a name="parameters"></a>參數
> 
> | 參數 |                                                                                    描述                                                                                    |
> |-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | 深度 = <n> | 指出要合併在一起的父系 VHD 檔案數目。 例如，**深度 = 1**表示差異 VHD 將與差異鏈的一個層級合併。 |
> 
> ## <a name="remarks"></a>備註
> - 必須選取並卸離 VHD，此作業才能成功。 使用 [**選取 vdisk** ] 命令來選取 VHD，並將焦點移至它。
> - 此參數會修改父 VHD。 因此，相依于父系的其他差異 Vhd 將不再有效。
>   ## <a name="BKMK_Examples"></a>典型
>   若要將差異 VHD 與父 VHD 合併，請輸入：
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>其他參考
> - [命令列語法關鍵](command-line-syntax-key.md)
> - [附加 vdisk](attach-vdisk.md)
> - [compact vdisk](compact-vdisk.md)

-   [詳細資料 vdisk](detail-vdisk.md)
-   [卸離 vdisk](detach-vdisk.md)
-   [展開 vdisk](expand-vdisk.md)
-   [選取 vdisk](select-vdisk.md)
-   [list_1](list_1.md)
