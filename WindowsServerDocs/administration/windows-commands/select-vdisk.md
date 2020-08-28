---
title: select vdisk
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 388804d8b895e636937050c6d04cae02cd30a0e8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032311"
---
# <a name="select-vdisk"></a>select vdisk

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的虛擬硬碟 \( VHD \) ，並將焦點移至該 VHD。

> [!NOTE]
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。

## <a name="syntax"></a>語法

```
select vdisk file=<full path> [noerr]
```

### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|檔\=<full path>|指定現有 VHD 檔案的完整路徑和檔案名。|
|noerr|僅供腳本使用。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。|

## <a name="examples"></a>範例
若要將焦點移至名為 Test 的 VHD，請輸入：

```
select vdisk file=c:\test\test.vhd
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

-   [附加 vdisk](attach-vdisk.md)

-   [compact vdisk](compact-vdisk.md)



-   [卸離 vdisk](detach-vdisk.md)

-   [detail vdisk](detail-vdisk.md)

-   [expand vdisk](expand-vdisk.md)

-   [Merge vdisk](merge-vdisk.md)

-   [list_1](./list.md)