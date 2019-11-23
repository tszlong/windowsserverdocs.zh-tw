---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: Fsutil wim
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: fc79b70e8dedb9ecad5e8c6e89f51ece3279faa4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376665"
---
# <a name="fsutil-wim"></a>Fsutil wim
>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

提供探索和管理 Windows 映像（WIM）支援之檔案的功能。

## <a name="syntax"></a>語法

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

### <a name="parameters"></a>Parameters

|參數|描述|
|-------------|---------------|
|enumfiles|列舉支援 WIM 的檔案。|
|\<磁片磁碟機名稱 >|指定磁片磁碟機名稱。|
|\<資料來源 >|指定資料來源。|
|enumwims|列舉支援的 WIM 檔案。|
|queryfile|查詢檔案是否受到 WIM 的支援，如果是，則會顯示 WIM 檔案的詳細資料。|
|\<filename >|指定檔案名。|
|removewim|從備份檔案移除 WIM。|




### <a name="examples"></a>範例

若要從資料來源0列舉磁片磁碟機 C：的檔案，請輸入：

```
fsutil wim enumfiles C: 0
```

若要列舉磁片磁碟機 C：的支援 WIM 檔案，請輸入：

```
fsutil wim enumwims C:
```

若要查看檔案是否受到 WIM 的支援，請輸入：

```
fsutil wim C:\Windows\Notepad.exe
```

若要從磁片區 C：和資料來源2的備份檔案中移除 WIM，請輸入：

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)