---
title: compact vdisk
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c4a95354bf041777e43a9eac5f16e693b02c185c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434301"
---
# <a name="compact-vdisk"></a>compact vdisk

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

縮減實體大小的動態擴充虛擬硬碟 (VHD) 檔案。 因為動態擴充 Vhd 大小增加，您將新增的檔案，但它們的確不會自動減少大小中刪除檔案時，這個參數很有用。
> [!NOTE]
> 此命令僅適用於 Windows 7 和 Windows Server 2008 R2。
> ## <a name="syntax"></a>語法
> ```
> compact vdisk
> ```
> ## <a name="remarks"></a>備註
> - 動態擴充 VHD 必須選取此作業才會成功。 使用**選取 vdisk**命令來選取 VHD，並將焦點移到它。
> - 此外，您只可以壓縮動態擴充 Vhd 卸離或附加為唯讀。
>   ## <a name="BKMK_Examples"></a>範例
>   若要壓縮動態擴充 VHD，請輸入：
>   ```
>   compact vdisk
>   ```
>   ## <a name="additional-references"></a>其他參考資料
> - [命令列語法關鍵](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [中斷連結 vdisk](detach-vdisk.md)
-   [展開 vdisk](expand-vdisk.md)
-   [合併 vdisk](merge-vdisk.md)
-   [選取 vdisk](select-vdisk.md)
-   [list_1](list_1.md)
