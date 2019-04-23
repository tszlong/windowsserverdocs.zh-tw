---
title: 展開 vdisk
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf8fd0b05ca5baeeee4fadd670adb3169130d04e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837399"
---
# <a name="expand-vdisk"></a>展開 vdisk

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以展開虛擬硬碟 (VHD) 以您指定的大小。
> [!NOTE]
> 此命令僅適用於 Windows 7 和 Windows Server 2008 R2。
## <a name="syntax"></a>語法
```
expand vdisk maximum=<n>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|maximum=<n>|指定新的大小 (mb) 中的 vhd。|
## <a name="remarks"></a>備註
-   必須選取 VHD，並將其卸離此作業才會成功。 使用**選取 vdisk**命令來選取磁碟區，並將焦點移到它。
## <a name="BKMK_Examples"></a>範例
若要展開選取的 VHD 為 20 GB，請輸入：
```
expand vdisk maximum=20000
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)
-   [attach vdisk](attach-vdisk.md)
-   [compact vdisk](compact-vdisk.md)

-   [中斷連結 vdisk](detach-vdisk.md)
-   [detail vdisk](detail-vdisk.md)
-   [合併 vdisk](merge-vdisk.md)
-   [選取 vdisk](select-vdisk.md)
-   [list_1](list_1.md)
