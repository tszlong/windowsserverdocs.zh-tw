---
title: detach vdisk
description: 卸離 vdisk 命令的參考文章，此命令會停止選取的虛擬硬碟 (VHD) ，使其不會顯示為主電腦上的本機硬碟機。
ms.topic: reference
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8f1fd81b4d68d471e07c461fd11ecb9d910745bc
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027696"
---
# <a name="detach-vdisk"></a>detach vdisk

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停止選取的虛擬硬碟 (VHD) ，使其不會顯示為主電腦上的本機硬碟機。 卸離 VHD 後，您可以將它複製到其他位置。 開始之前，您必須先選取 VHD，此作業才會成功。 使用 [ [選取 vdisk](select-vdisk.md) ] 命令選取 VHD，並將焦點移至該 VHD。


## <a name="syntax"></a>語法

```
detach vdisk [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要卸離選取的 VHD，請輸入：

```
detach vdisk
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [attach vdisk 命令](attach-vdisk.md)

- [compact vdisk 命令](compact-vdisk.md)

- [detail vdisk 命令](detail-vdisk.md)

- [展開 [vdisk] 命令](expand-vdisk.md)

- [Merge vdisk 命令](merge-vdisk.md)

- [選取 vdisk 命令](select-vdisk.md)

- [list 命令](list.md)
