---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: fsutil 配額
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 2daa2f6253a406ccad68677e1877215a1c610328
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835289"
---
# <a name="fsutil-quota"></a>fsutil 配額
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

管理磁碟配額，以提供更精確地控制網路型存放區的 NTFS 磁碟區上。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil quota [disable] <VolumePath>
fsutil quota [enforce] <VolumePath>
fsutil quota [modify] <VolumePath> <Threshold> <Limit> <UserName>
fsutil quota [query] <VolumePath>
fsutil quota [track] <VolumePath>
fsutil quota [violations]
```

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|停用|停用追蹤的配額和指定的磁碟區上的強制執行。|
|強制執行|會強制執行指定的磁碟區上的配額使用量。|
|modify|修改現有的磁碟配額，或建立新的配額。|
|查詢|列出現有的磁碟配額。|
|追蹤|追蹤磁碟上指定的磁碟區的使用量。|
|違規|搜尋系統和應用程式記錄檔，並顯示訊息來指出，偵測到配額違規或使用者已達到配額閾值或配額限制。|
|\<VolumePath>|必要。 指定磁碟機名稱，後面接著冒號或格式的 GUID**磁碟區 {***GUID***}**。|
|\<臨界值 >|設定的限制 （以位元組為單位） 發出警告。 這是必要的參數**fsutil 配額修改**命令。|
|\<限制 >|設定最大允許的磁碟使用量 （以位元組為單位）。 這是必要的參數**fsutil 配額修改**命令。|
|\<UserName>|指定網域或使用者名稱。 這是必要的參數**fsutil 配額修改**命令。|

## <a name="remarks"></a>備註

-   以每個磁碟區為基礎，實作磁碟配額，而且它們可讓這兩個實作以每個使用者為基礎的硬性和軟性儲存體限制。

-   您可以使用撰寫指令碼使用**fsutil 配額**若要設定的配額限制，每次您新增使用者或自動追蹤配額限制，插入報表時，將它們編譯並自動將它們傳送至電子郵件中的系統管理員。

### <a name="BKMK_examples"></a>範例
若要列出現有的磁碟的磁碟區 GUID，{928842df-5a01-11de-a85c-806e6f6e6963}，使用指定的配額類型：

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

若要列出的磁碟區指定磁碟機代號，現有的磁碟配額**c:**，型別：

```
Fsutil quota query C:
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


