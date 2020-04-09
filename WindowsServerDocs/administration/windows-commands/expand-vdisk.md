---
title: 展開 vdisk
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 272714372a35f7f205b5a2e70cb2f2669b3a0634
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844891"
---
# <a name="expand-vdisk"></a>展開 vdisk

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將虛擬硬碟（VHD）擴充為您指定的大小。
> [!NOTE]
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。
> ## <a name="syntax"></a>語法
> ```
> expand vdisk maximum=<n>
> ```
> ### <a name="parameters"></a>參數
> 
> |  參數  |                      描述                      |
> |-------------|-------------------------------------------------------|
> | 最大值 =<n> | 指定 VHD 的新大小（以 mb 為單位）。 |
> 
> ## <a name="remarks"></a>備註
> - 必須選取並卸離 VHD，此作業才能成功。 使用 [**選取 vdisk** ] 命令來選取磁片區，並將焦點移至它。
>   ## <a name="examples"></a><a name=BKMK_Examples></a>典型
>   若要將選取的 VHD 展開為 20 GB，請輸入：
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>其他參考資料
> - - [命令列語法關鍵](command-line-syntax-key.md)
> - [附加 vdisk](attach-vdisk.md)
> - [compact vdisk](compact-vdisk.md)

-   [卸離 vdisk](detach-vdisk.md)
-   [詳細資料 vdisk](detail-vdisk.md)
-   [合併 vdisk](merge-vdisk.md)
-   [選取 vdisk](select-vdisk.md)
-   [list_1](list_1.md)
