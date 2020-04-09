---
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
title: Fsutil 分層
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 8227fafc6b29471e2f09db171645012967553429
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844051"
---
# <a name="fsutil-tiering"></a>Fsutil 分層
>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

啟用儲存層功能的管理，例如設定和停用旗標和層級清單。

## <a name="syntax"></a>語法

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

#### <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|clearflags|停用磁片區的分層行為旗標。|
|\<磁片區 >|指定磁片區。|
|/TrNH|針對具有階層式存放裝置的磁片區，將會停用熱收集。<br /><br>僅適用于 NTFS 和 ReFS。|
|queryflags|查詢磁片區的分層行為旗標。|
|regionlist|列出磁片區的分層區域及其各自的儲存層。|
|setflags|啟用磁片區的分層行為旗標。|
|tierlist|列出與磁片區相關聯的儲存體 tieres。|


### <a name="examples"></a>範例

若要查詢磁片區 C 上的旗標，請輸入：

```
fsutil tiering clearflags C:
```

若要在磁片區 C 上設定旗標，請輸入：

```
fsutil tiering setflags C: /TrNH
```

若要清除磁片區 C 上的旗標，請輸入：

```
fsutil tiering clearflags C: /TrNH
```

若要列出磁片區 C 的區域及其各自的儲存層，請輸入：

```
fsutil tiering regionlist C:
```

若要列出磁片區 C 的層級，請輸入：

```
fsutil tiering tierlist C:
```



### <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

