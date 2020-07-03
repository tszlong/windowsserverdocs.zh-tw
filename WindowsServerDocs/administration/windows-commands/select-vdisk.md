---
title: select vdisk
description: '* * * * 的參考文章'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 734b31d9c9bcf174bf4617418978935bc49ad6da
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936433"
---
# <a name="select-vdisk"></a>select vdisk

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

選取指定的虛擬硬碟 \( VHD \) ，並將焦點轉移到其上。

> [!NOTE]
> 此命令僅適用于 Windows 7 和 Windows Server 2008 R2。

## <a name="syntax"></a>語法

```
select vdisk file=<full path> [noerr]
```

### <a name="parameters"></a>參數

|參數|說明|
|-------|--------|
|文字檔\=<full path>|指定現有 VHD 檔案的完整路徑和檔案名。|
|noerr|僅用於腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="examples"></a>範例
若要將焦點移至名為 Test .vhd 的 VHD，請輸入：

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

-   [合併 vdisk](merge-vdisk.md)

-   [list_1](list_1.md)


