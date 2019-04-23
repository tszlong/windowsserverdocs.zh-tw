---
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
title: fsutil fsinfo
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 434dfde2286538367fb96d168b06983cb4357067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873039"
---
# <a name="fsutil-fsinfo"></a>fsutil fsinfo
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

列出所有磁碟機、 磁碟機類型的查詢、 查詢磁碟區資訊、 查詢特定 NTFS 的磁碟區資訊，或查詢檔案系統的統計資料。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <VolumePath>
fsutil fsinfo [ntfsinfo] <RootPath>
fsutil fsinfo [statistics] <VolumePath>
fsutil fsinfo [volumeinfo] <RootPath>
```

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|磁碟機|列出在電腦中的所有磁碟機。|
|drivetype|查詢磁碟機，並列出其類型，例如 CD-ROM 光碟機。|
|ntfsinfo|列出指定的磁碟區，例如磁區、 總叢集、 可用的叢集，以及開始和結尾 MFT 區域數目的 NTFS 特定磁碟區資訊。|
|sectorinfo|列出的硬體磁區大小和對齊方式的相關資訊。|
|統計資料|清單的檔案系統為指定的磁碟區，例如中繼資料、 記錄檔和 MFT 讀取和寫入的統計資料。|
|volumeinfo|列出指定的磁碟區，例如檔案系統的資訊，而且磁碟區支援區分大小寫的檔案名稱、 檔案名稱中的 unicode 是否磁碟配額，或 DirectAccess (DAX) 磁碟區。|
|<"VolumePath">|指定的磁碟機代號 （後面接著冒號）。|
|<"RootPathname">|指定根磁碟機的磁碟機代號 （後面接著冒號）。|

## <a name="BKMK_examples"></a>範例
若要列出所有電腦中的磁碟機，請輸入：

```
fsutil fsinfo drives
```

輸出會顯示下列類似：

```
Drives: A:\ C:\ D:\ E:\       
```

若要查詢的 C 磁碟機的磁碟機類型，請輸入：

```
fsutil fsinfo drivetype c:
```

查詢的可能結果包括：

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

若要查詢磁碟區 E 的磁碟區資訊，請輸入：

```
fsinfo volumeinfo e:\
```

輸出會顯示下列類似：

```
Volume Name :Volume
Serial Number : 0xd0b634d9
Max Component Length : 255
File System Name : NTFS
.
.
.
Supports Named Streams
Is DAX Volume
```

若要查詢磁碟 F 的特定 NTFS 的磁碟區資訊，請輸入：

```
fsutil fsinfo ntfsinfo f:
```

輸出會顯示下列類似：

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors :            0x00000000010ea04f
Total Clusters :            0x000000000021d409
.
.
.
Mft Zone End   :            0x0000000000004700       
```

若要查詢檔案系統的基礎硬體的磁區資訊，請輸入：

```
fsinfo sectorinfo d:
```

輸出會顯示下列類似：

```
D:\>fsutil fsinfo sectorinfo d:
LogicalBytesPerSector :                                 4096
PhysicalBytesPerSectorForAtomicity :                    4096
.
.
.
Trim Not Supported
DAX capable
```

若要查詢 E 磁碟機的檔案系統統計資料，請輸入：

```
fsinfo statistics e:
```

輸出會顯示下列類似：

```
File System Type :     NTFS
Version :              1
UserFileReads :        75021
UserFileReadBytes :    1305244512
.
.
.
LogFileWriteBytes :    180936704       
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](Command-Line-Syntax-Key.md)
[Fsutil](Fsutil.md)


