---
title: detach vdisk
description: 卸離 vdisk 命令的參考文章，這會停止選取的虛擬硬碟 (VHD) 不會顯示為主電腦上的本機硬碟。
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1efe62064a25d72eacf7175be287a32b7670e917
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891322"
---
# <a name="detach-vdisk"></a>detach vdisk

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停止選取的虛擬硬碟 (VHD) 不會顯示為主電腦上的本機硬碟。 卸離 VHD 之後，您可以將它複製到其他位置。 開始之前，您必須選取 VHD，此作業才會成功。 使用 [[選取 vdisk](select-vdisk.md) ] 命令來選取 VHD，並將焦點移至它。


## <a name="syntax"></a>語法

```
detach vdisk [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="examples"></a>範例

若要卸離選取的 VHD，請輸入：

```
detach vdisk
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [附加 vdisk 命令](attach-vdisk.md)

- [compact vdisk 命令](compact-vdisk.md)

- [detail vdisk 命令](detail-vdisk.md)

- [展開 vdisk 命令](expand-vdisk.md)

- [Merge vdisk 命令](merge-vdisk.md)

- [選取 vdisk 命令](select-vdisk.md)

- [list 命令](list.md)
