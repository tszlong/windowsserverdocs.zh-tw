---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: Fsutil wim
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c9186721ce4d3a549964e420cbc16d4893a1859d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826039"
---
# <a name="fsutil-wim"></a>Fsutil wim
>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows 10

提供探索及管理 Windows 映像 WIM 備份檔案的函式。

## <a name="syntax"></a>語法

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

### <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|enumfiles|列舉支援的 WIM 檔案。|
|\<磁碟機名稱 >|指定磁碟機名稱。|
|\<資料來源 >|指定的資料來源。|
|enumwims|列舉支援 WIM 檔案。|
|queryfile|如果檔案做為後盾 WIM，而且如果是的話，會顯示 WIM 檔案詳細資料，查詢。|
|\<filename>|指定的檔案名稱。|
|removewim|請移除備份檔案中的 WIM。|




### <a name="examples"></a>範例

若要列舉磁碟機 c： 從資料來源為 0 的檔案，請輸入：

```
fsutil wim enumfiles C: 0
```

若要列舉的磁碟機 c： 支援 WIM 檔案，請輸入：

```
fsutil wim enumwims C:
```

若要查看檔案的 WIM 為後盾，請輸入：

```
fsutil wim C:\Windows\Notepad.exe
```

若要移除 WIM 備份磁碟區 c： 與資料來源 2 的檔案，請輸入：

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)