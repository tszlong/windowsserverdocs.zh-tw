---
title: 合併 vdisk
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bc20fcaf6e511bb25156996bddc3357f99195875
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437416"
---
# <a name="merge-vdisk"></a>合併 vdisk

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

合併差異虛擬硬碟 (VHD) 與其對應的父代 VHD。 父代 VHD 將會修改為包含從差異 VHD 的修改。
> [!NOTE]
> 此命令僅適用於 Windows 7 和 Windows Server 2008 R2。
> ## <a name="syntax"></a>語法
> ```
> merge vdisk depth=<n>
> ```
> ### <a name="parameters"></a>參數
> 
> | 參數 |                                                                                    描述                                                                                    |
> |-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | depth=<n> | 表示要合併在一起的父代 VHD 檔案的數目。 例如，**深度 = 1**指出差異 VHD 將會合併差異鏈結的一層級。 |
> 
> ## <a name="remarks"></a>備註
> - 必須選取 VHD，並將其卸離此作業才會成功。 使用**選取 vdisk**命令來選取 VHD，並將焦點移到它。
> - 此參數可修改父 VHD。 如此一來，其他相依於父代的差異 Vhd 將不再有效。
>   ## <a name="BKMK_Examples"></a>範例
>   若要合併差異 VHD 隨其父代 VHD，請輸入：
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>其他參考資料
> - [命令列語法關鍵](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)
> - [compact vdisk](compact-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [中斷連結 vdisk](detach-vdisk.md)
-   [展開 vdisk](expand-vdisk.md)
-   [選取 vdisk](select-vdisk.md)
-   [list_1](list_1.md)
