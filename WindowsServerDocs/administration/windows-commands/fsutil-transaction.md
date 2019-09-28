---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: Fsutil transaction
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 286660baad699e21abe751a9cb956b1ac7613e80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376865"
---
# <a name="fsutil-transaction"></a>Fsutil transaction
>適用於：Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7、Windows 2008、Windows Vista

管理 NTFS 交易。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <Filename>
fsutil transaction [list]
fsutil transaction [query] [{Files|All}] <GUID>
fsutil transaction [rollback] <GUID>
```

### <a name="parameters"></a>參數

| 參數  |                                                                                                                                                     描述                                                                                                                                                     |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   Commit   |                                                                                                                      標示成功的隱含或明確指定交易的結尾。                                                                                                                      |
|   <GUID>   |                                                                                                                               指定代表交易的 GUID 值。                                                                                                                               |
|  fileinfo  |                                                                                                                              顯示指定檔案的交易資訊。                                                                                                                               |
| <Filename> |                                                                                                                                         指定完整路徑和檔案名。                                                                                                                                          |
|    list    |                                                                                                                                 顯示目前正在執行的交易清單。                                                                                                                                  |
|   query    | 顯示指定之交易的資訊。<br /><br />-如果指定了**fsutil transaction query**檔案，則只會針對指定的交易顯示檔案資訊。<br />-如果指定了**fsutil transaction Query all** ，就會顯示該交易的所有資訊。 |
|  回退  |                                                                                                                                將指定的交易回復到一開始。                                                                                                                                 |

### <a name="remarks"></a>備註

-   交易式 NTFS 是在 Windows Server 2008 中引進。

### <a name="BKMK_examples"></a>典型
若要顯示檔案 c:\test.txt 的交易資訊，請輸入：

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[交易式 NTFS](https://go.microsoft.com/fwlink/?LinkID=165402)


