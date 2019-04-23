---
title: 管理虛擬硬碟 (VHD)
description: 本文說明如何管理虛擬硬碟
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2e371710752d59ebc7a1f8aa2dad3d9189872c47
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827109"
---
# <a name="manage-virtual-hard-disks-vhd"></a>管理虛擬硬碟 (VHD)

> **適用於：** Windows 10，Windows 8.1、 Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主題說明如何使用 [磁碟管理] 建立、連結和中斷連結虛擬硬碟。 虛擬硬碟 (VHD) 是虛擬化硬碟檔案，一經掛接，就差不多和實際硬碟一模一樣地顯示和運作。 它們最常與 Hyper-V 虛擬機器搭配使用。 

## <a name="viewing-vhds-in-disk-management"></a>在磁碟管理中檢視 VHD

VHD 就像實體磁碟一樣顯示在 [磁碟管理] 中。 VHD 連結後 (也就是，提供給系統使用)，會顯示藍色。 如果磁碟中斷連結 (也就是，變得無法使用)，其圖示會回復為灰色。

## <a name="creating-a-vhd"></a>建立 VHD

> [!NOTE]
> 您必須至少是**備份操作員**或**系統管理員**群組的成員，才能完成這些步驟。

**若要建立 VHD**

1.  在 **\[動作\]** 功能表上，選取 **\[建立 VHD\]**。

2.  在 **\[建立和連結虛擬硬碟\]** 對話方塊中，同時指定您想要在實體電腦上儲存 VHD 檔案的位置，以及 VHD 的大小。

3.  在 **\[虛擬硬碟格式\]** 中，選取 **\[動態擴充\]** 或 **\[固定大小\]**，然後按一下 **\[確定\]**。

## <a name="attaching-and-detaching-a-vhd"></a>連結和中斷連結 VHD

若要讓 VHD 可供使用 (無論是剛建立的還是其他現有的 VHD)： 

1. 在 **\[動作\]** 功能表上，選取 **\[連結 VHD\]**。

2. 使用完整路徑指定 VHD 的位置。

若要中斷連結 VHD，使其無法使用：以滑鼠右鍵按一下磁碟，請選取**中斷連結 VHD**，然後按一下**確定**。 中斷連結 VHD 不會刪除 VHD 或其中儲存的任何資料。

## <a name="additional-considerations"></a>其他考量

-   VHD 必須完整，而且不能在指定位置的路徑\\Windows 目錄。
-   VHD 的大小下限是 3 MB。
-   VHD 只能是基本磁碟。
-   由於 VHD 是在建立時初始化，建立大型固定大小的 VHD 可能需要一些時間。
