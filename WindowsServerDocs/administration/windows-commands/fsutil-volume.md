---
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
title: Fsutil volume
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c4496cfec94823ae177bc6de4fac83dc977fb61d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376703"
---
# <a name="fsutil-volume"></a>Fsutil volume
>適用於：Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

卸載磁片區，或查詢硬碟以判斷硬碟上目前有多少可用空間，或哪些檔案正在使用特定的叢集。

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
|allocationreport|顯示如何在指定磁片區上使用儲存體的相關資訊。|
|\<VolumePath >|指定磁碟機號（後面接著冒號）。|
|diskfree|查詢硬碟以判斷磁片上的可用空間量。|
|卸載 - dismount|卸載磁片區。|
|filelayout|顯示指定檔案的 NTFS 中繼資料。|
|\<fileid >|指定檔案識別碼。|
|list|列出系統上的所有磁片區。|
|querycluster|尋找哪個檔案正在使用指定的叢集。 您可以使用**querycluster**參數指定多個叢集。<br /><br />此參數適用于：Windows Server 2008 R2 和 Windows 7。|
|\<cluster >|指定邏輯群集編號（LCN）。|

## <a name="BKMK_examples"></a>典型
若要顯示已配置的叢集報告，請輸入：

```
fsutil volume allocationreport C:
```

若要卸載磁片磁碟機 C 上的磁片區，請輸入：

```
fsutil volume dismount c:
```

若要查詢磁片磁碟機 C 上磁片區的可用空間量，請輸入：

```
fsutil volume diskfree c:
```

若要顯示指定檔案的所有相關資訊，請輸入：

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

若要列出磁片上的磁片區，請輸入：

```
fsutil volume list
```

若要尋找使用叢集（由邏輯叢集編號50和0x2000 指定）的檔案，請在磁片磁碟機 C 上輸入：

```
fsutil volume querycluster C: 50 0x2000
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS 的運作方式](https://go.microsoft.com/fwlink/?LinkId=183396)


