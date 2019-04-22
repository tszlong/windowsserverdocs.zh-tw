---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: Fsutil 交易
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 93c981d077dbb027400a1eb2e2c662f72c14cc44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825209"
---
# <a name="fsutil-transaction"></a>Fsutil 交易
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7、 Windows 2008，Windows Vista

管理 NTFS 的交易。

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

|參數|描述|
|-------------|---------------|
|認可|將標記成功的隱含或明確指定交易的結尾。|
|<GUID>|指定代表交易的 GUID 值。|
|fileinfo|顯示指定的檔案的交易資訊。|
|<Filename>|指定完整路徑和檔案名稱。|
|list|顯示目前執行中交易的清單。|
|查詢|顯示指定交易的資訊。<br /><br />-如果**fsutil 交易查詢檔**指定，會顯示檔案資訊，只會針對指定的交易。<br />-如果**fsutil 交易查詢所有**指定，則會顯示交易的所有資訊。|
|復原|開頭會回復指定的交易。|

### <a name="remarks"></a>備註

-   Windows Server 2008 中引進了交易式 NTFS。

### <a name="BKMK_examples"></a>範例
若要顯示檔案 c:\test.txt 的交易資訊，請輸入：

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[交易式 NTFS](https://go.microsoft.com/fwlink/?LinkID=165402)


