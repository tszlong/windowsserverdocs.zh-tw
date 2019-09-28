---
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
title: Fsutil 硬連結
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: de9a04850555b11a42d74826685c249bea15710c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376883"
---
# <a name="fsutil-hardlink"></a>Fsutil 硬連結
>適用於：Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

建立現有檔案與新檔案之間的永久連結。

## <a name="syntax"></a>語法

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|create|在現有檔案和新檔案之間建立 NTFS 永久連結。 （NTFS 硬連結類似 POSIX 硬連結）。|
|\<NewFileName >|指定您想要建立硬連結的檔案。|
|\<ExistingFileName >|指定您想要建立硬連結的檔案。|
|list|列出永久連結為*檔案名*。<br /><br />此參數適用于：Windows Server 2008 R2 和 Windows 7。|

## <a name="remarks"></a>備註

-   「硬連結」是檔案的目錄專案。 每個檔案都可以被視為至少有一個硬連結。 在 NTFS 磁片區上，每個檔案都可以有多個永久連結，因此單一檔案可能會出現在許多目錄中（或甚至在具有不同名稱的相同目錄中）。 由於所有連結都參考相同的檔案，因此程式可以開啟任何連結並修改檔案。 只有在刪除檔案的所有連結之後，檔案才會從檔案系統中刪除。 建立硬式連結之後，程式就可以像任何其他檔案名一樣使用它。

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


