---
title: 管理虛擬硬碟 (VHD)
description: 本文描述如何管理虛擬硬碟
ms.date: 10/12/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6ffa7e9dc769b8d8c892d0af1ceae5246df62d3e
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "71385815"
---
# <a name="manage-virtual-hard-disks-vhd"></a>管理虛擬硬碟 (VHD)

> **適用於：** Windows 10、Windows 8.1、Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題描述如何使用 [磁碟管理] 建立、連結和中斷連結虛擬硬碟。 虛擬硬碟 (VHD) 是虛擬化硬碟檔案，一經掛接就會幾乎如同實體硬碟一般地顯示和運作。 它們最常與 Hyper-V 虛擬機器搭配使用。 

## <a name="viewing-vhds-in-disk-management"></a>在磁碟管理中檢視 VHD

VHD 如同實體磁碟一般地顯示在 [磁碟管理] 中。 VHD 連結後 (也就是，提供給系統使用) 會顯示藍色。 如果磁碟中斷連結 (也就是，變得無法使用)，其圖示會回復為灰色。

## <a name="creating-a-vhd"></a>建立 VHD

> [!NOTE]
> 您必須至少是**備份操作員**或**系統管理員**群組的成員，才能完成這些步驟。

**建立 VHD**

1.  在 [動作]  功能表上，選取 [建立 VHD]  。

2.  在 [建立並連結虛擬硬碟]  對話方塊中，同時指定您想要在實體電腦上儲存 VHD 檔案的位置及 VHD 大小。

3.  在 [虛擬硬碟格式]  中，選取 [動態擴充]  或 [固定大小]  ，然後按一下 [確定]  。

## <a name="attaching-and-detaching-a-vhd"></a>連結和中斷連結 VHD

若要讓 VHD 可供使用 (無論是剛建立或其他現有的 VHD)： 

1. 在 [動作]  功能表上，選取 [連結 VHD]  。

2. 使用完整路徑指定 VHD 的位置。

若要中斷連結 VHD，使其無法供使用：以滑鼠右鍵按一下磁碟，選取 [中斷連結 VHD]  ，然後按一下 [確定]  。 中斷連結 VHD 不會刪除 VHD 或其中儲存的任何資料。

## <a name="additional-considerations"></a>其他考量

-   指定 VHD 位置的路徑必須是完整路徑，且不可在 \\Windows 目錄中。
-   VHD 的大小下限是 3 MB。
-   VHD 只能是基本磁碟。
-   由於 VHD 是在建立時初始化，因此建立固定大小的大型 VHD 可能需要一些時間。
