---
title: expand vdisk
description: Expand vdisk 命令的參考文章，可將虛擬硬碟 (VHD) 擴充為指定的大小。
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d9c5c859dfa506d9f8afd07a0e78bdef210f60a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890446"
---
# <a name="expand-vdisk"></a>expand vdisk

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將 (VHD) 的虛擬硬碟擴充為指定的大小。

必須選取並卸離 VHD，此作業才能成功。 使用 [[選取 vdisk] 命令](select-vdisk.md)來選取磁片區，並將焦點移至它。

## <a name="syntax"></a>語法

```
expand vdisk maximum=<n>
```

### <a name="parameters"></a>參數

 | 參數 | 描述 |
 |---------- | ----------- |
 | 最大值 =`<n>` | 指定 VHD 的新大小（以 mb 為單位） (MB) 。 |

### <a name="examples"></a>範例

若要將選取的 VHD 展開為 20 GB，請輸入：

```
expand vdisk maximum=20000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [選取 vdisk 命令](select-vdisk.md)

- [附加 vdisk 命令](attach-vdisk.md)

- [compact vdisk 命令](compact-vdisk.md)

- [卸離 vdisk 命令](detach-vdisk.md)

- [detail vdisk 命令](detail-vdisk.md)

- [merge vdisk 命令](merge-vdisk.md)

- [list 命令](list.md)
