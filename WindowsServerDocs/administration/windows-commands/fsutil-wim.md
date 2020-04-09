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
ms.openlocfilehash: d4a8f2c008c1a28e498edb7726a8c209e91f41af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843921"
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

#### <a name="parameters"></a>參數

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
- [命令列語法關鍵](command-line-syntax-key.md)

[Fsutil](Fsutil.md)