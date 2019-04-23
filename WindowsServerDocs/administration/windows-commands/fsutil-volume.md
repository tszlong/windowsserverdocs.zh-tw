---
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
title: fsutil 磁碟區
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: a8576dce4be639a516f8898e78bb6db12c91e171
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882449"
---
# <a name="fsutil-volume"></a>fsutil 磁碟區
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

卸載磁碟區，或查詢來決定哪個檔案使用特定的叢集或多少可用空間的硬碟機上目前可用的硬碟。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil volume [allocationreport] <VolumePath>
fsutil volume [diskfree] <VolumePath>
fsutil volume [dismount] <VolumePath>
fsutil volume [filelayout] <VolumePath> <fileid>
fsutil volume [list]
fsutil volume [querycluster] <VolumePath> <Cluster> [<Cluster>] … …
```

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|allocationreport|顯示指定的磁碟區上使用儲存體的相關資訊。|
|\<VolumePath>|指定的磁碟機代號 （後面接著冒號）。|
|diskfree|查詢以判斷在其上的可用空間數量的硬碟。|
|卸載 - dismount|卸載磁碟區。|
|filelayout|顯示指定檔案的 NTFS 中繼資料。|
|\<fileid>|指定的檔案識別碼。|
|list|列出所有系統上的磁碟區。|
|querycluster|找出哪些檔案使用指定的叢集。 您可以指定多個叢集與**querycluster**參數。<br /><br />此參數適用於：Windows Server 2008 R2 和 Windows 7。|
|\<cluster>|指定邏輯叢集數目 (LCN)。|

## <a name="BKMK_examples"></a>範例
若要顯示配置的叢集報表，請輸入：

```
fsutil volume allocationreport C:
```

若要卸載磁碟機 C 上的磁碟區，請輸入：

```
fsutil volume dismount c:
```

若要查詢 C 磁碟機上的磁碟區的可用空間數量，請輸入：

```
fsutil volume diskfree c:
```

若要顯示指定檔案的所有資訊，請輸入：

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

若要列出的磁碟上的磁碟區，請輸入：

```
fsutil volume list
```

若要尋找使用叢集，邏輯的叢集數字 50 和 0x2000，C，磁碟機上的所指定的檔案中，輸入：

```
fsutil volume querycluster C: 50 0x2000
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS 的運作方式](https://go.microsoft.com/fwlink/?LinkId=183396)


