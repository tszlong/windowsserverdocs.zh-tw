---
title: select vdisk
description: Select vdisk 命令的參考文章，此命令會選取指定的虛擬硬碟 (VHD) ，並將焦點移至其中。
ms.topic: reference
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 19e842115f914711cc59cfb770ee19f732b3a388
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389204"
---
# <a name="select-vdisk"></a>select vdisk

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的虛擬硬碟 \( VHD \) ，並將焦點移至該 VHD。

## <a name="syntax"></a>語法

```
select vdisk file=<full path> [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| file =`<full path>` | 指定現有 VHD 檔案的完整路徑和檔案名。 |
| noerr | 僅供腳本使用。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要將焦點移至名為 *c:\test\test.vhd*的 VHD，請輸入：

```
select vdisk file=c:\test\test.vhd
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [附加 vdisk](attach-vdisk.md)

- [compact vdisk](compact-vdisk.md)

- [detach vdisk](detach-vdisk.md)

- [detail vdisk](detail-vdisk.md)

- [expand vdisk](expand-vdisk.md)

- [merge vdisk](merge-vdisk.md)

- list

- [選取磁片命令](select-disk.md)

- [選取資料分割命令](select-partition.md)

- [選取磁片區命令](select-volume.md)