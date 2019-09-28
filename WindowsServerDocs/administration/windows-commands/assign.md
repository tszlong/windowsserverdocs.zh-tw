---
title: assign
description: '**指派**的 Windows 命令主題-將磁碟機號或掛接點指派給具有焦點的磁片區。'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e07edcd4ac4ddf5eca1e57da17df441043d15f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382683"
---
# <a name="assign"></a>assign

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將磁碟機號或掛接點指派給具有焦點的磁片區。

## <a name="syntax"></a>語法
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>參數

|  參數   |                                                                                                                                 描述                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  字母 = <d>  |                                                                                                             您想要指派給磁片區的磁碟機號。                                                                                                              |
| mount = <path> | 您想要指派給磁片區的掛接點路徑。<br /><br />如需有關如何使用此命令的指示，請參閱[將掛接點資料夾路徑指派給磁片磁碟機](https://go.microsoft.com/fwlink/?LinkId=207059)（<https://go.microsoft.com/fwlink/?LinkId=207059>）。 |
|    noerr     |                                    僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。                                     |

## <a name="remarks"></a>備註
- 如果未指定磁碟機號或掛接點，則會指派下一個可用的磁碟機號。 如果磁碟機號或掛接點已在使用中，就會產生錯誤。
- 藉由使用 [指派] 命令，您可以變更與卸載式磁片磁碟機相關聯的磁碟機號。
- 您不能將磁碟機號指派給系統磁碟區、開機磁碟區或包含分頁檔的磁片區。 此外，您不能將磁碟機號指派給原始設備製造商（OEM）磁碟分割，或是基本資料分割以外的任何 GUID 磁碟分割表格（gpt）磁碟分割。
- 必須選取磁片區，此操作才能成功。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。
  ## <a name="BKMK_examples"></a>典型
  若要將字母 E 指派給焦點的磁片區，請輸入：
  ```
  assign letter=e
  ```

