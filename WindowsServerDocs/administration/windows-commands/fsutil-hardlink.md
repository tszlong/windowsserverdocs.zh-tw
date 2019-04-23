---
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
title: Fsutil hardlink
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 69474bd1817471176598afba508cd80c8fa1df8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840049"
---
# <a name="fsutil-hardlink"></a>Fsutil hardlink
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

建立永久連結之間現有的檔案和新的檔案。

## <a name="syntax"></a>語法

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|建立|建立 NTFS 永久連結之間現有的檔案和新的檔案。 （NTFS 永久連結是類似的 POSIX 硬式連結）。|
|\<NewFileName>|指定您想要建立的硬式連結的檔案。|
|\<ExistingFileName>|指定您想要建立從硬式連結的檔案。|
|list|列出要永久*Filename*。<br /><br />此參數適用於：Windows Server 2008 R2 和 Windows 7。|

## <a name="remarks"></a>備註

-   永久連結是檔案的目錄項目。 每個檔案會被視為可以擁有一個以上的永久連結。 在 NTFS 磁碟區，每個檔案可以有多個的永久連結，因此單一檔案可以出現在多個目錄中 （或甚至是在相同的目錄有不同的名稱）。 由於所有的連結參考相同的檔案，程式就可以開啟任何一個連結，並修改檔案。 只有在已刪除所有連結之後，才從檔案系統都刪除檔案。 建立永久連結之後，程式可以使用它與任何其他的檔案名稱。

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


