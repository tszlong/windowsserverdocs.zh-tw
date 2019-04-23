---
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
title: fsutil 階層處理
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: dcb69e4e9c71a723bfd735eb7915472f1232a92b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859249"
---
# <a name="fsutil-tiering"></a>fsutil 階層處理
>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows 10

可管理的儲存體層功能，例如設定和停用旗標和層級的清單。

## <a name="syntax"></a>語法

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

### <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|clearflags|停用磁碟區的分層行為旗的標。|
|\<volume>|指定的磁碟區。|
|/TrNH|針對使用分層式儲存體的磁碟區，會導致要停用收集的熱度圖。<br /><br>僅適用於 NTFS 和 ReFS。|
|queryflags|查詢磁碟區的分層行為旗標。|
|regionlist|列出磁碟區和其個別的儲存層的分層式的區域。|
|setflags|可讓磁碟區的分層行為旗標。|
|tierlist|列出磁碟區相關聯的儲存體 tieres。|


### <a name="examples"></a>範例

若要查詢磁碟區 C 上的旗標，請輸入：

```
fsutil tiering clearflags C:
```

若要設定磁碟區 C 旗標，請輸入：

```
fsutil tiering setflags C: /TrNH
```

若要清除的磁碟區 C 上的旗標，請輸入：

```
fsutil tiering clearflags C: /TrNH
```

若要列出的磁碟區 C 和其個別的儲存層的區域，請輸入：

```
fsutil tiering regionlist C:
```

若要列出的磁碟區 C 層，請輸入：

```
fsutil tiering tierlist C:
```



### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

