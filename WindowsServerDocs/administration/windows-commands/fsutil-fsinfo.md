---
title: Fsutil fsinfo
description: Fsutil fsinfo 命令的參考主題，其中會列出所有磁片磁碟機、查詢磁片磁碟機類型、查詢磁片區資訊、查詢 NTFS 特定磁片區資訊，或查詢檔案系統統計資料。
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 04d64bf0d7d29290cfc5e1ca88a013432322dbc1
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435823"
---
# <a name="fsutil-fsinfo"></a>fsutil fsinfo

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

列出所有磁片磁碟機、查詢磁片磁碟機類型、查詢磁片區資訊、查詢 NTFS 特定磁片區資訊，或查詢檔案系統統計資料。

## <a name="syntax"></a>語法

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <volumepath>
fsutil fsinfo [ntfsinfo] <rootpath>
fsutil fsinfo [statistics] <volumepath>
fsutil fsinfo [volumeinfo] <rootpath>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |------------ |
| 磁碟機 | 列出電腦中的所有磁片磁碟機。 |
| drivetype | 查詢磁片磁碟機並列出其類型，例如 CD-ROM 光碟機。 |
| ntfsinfo | 列出指定磁片區的 NTFS 特定磁片區資訊，例如磁區數目、叢集總計、可用叢集，以及 MFT 區域的開始和結束。 |
| sectorinfo | 列出硬體磁區大小和對齊的相關資訊。 |
| 統計資料 | 列出指定磁片區（例如中繼資料、記錄檔和 MFT 讀取和寫入）的檔案系統統計資料。 |
| volumeinfo | 列出指定磁片區的資訊（例如檔案系統），以及該磁片區是否支援區分大小寫的檔案名、檔案名中的 unicode、磁片配額，或為 DirectAccess （DAX）磁片區。 |
| `<volumepath>:` | 指定磁碟機號（後面接著冒號）。 |
| `<rootpath>:` | 指定根磁片磁碟機的磁碟機號（後面接著冒號）。 |

### <a name="examples"></a>範例

若要列出電腦中的所有磁片磁碟機，請輸入：

```
fsutil fsinfo drives
```

輸出類似下列顯示：

```
Drives: A:\ C:\ D:\ E:\
```

若要查詢磁片磁碟機 C 的磁片磁碟機類型，請輸入：

```
fsutil fsinfo drivetype c:
```

可能的查詢結果包括：

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

若要查詢磁片區 E 的磁片區資訊，請輸入：

```
fsinfo volumeinfo e:\
```

輸出類似下列顯示：

```
Volume Name : Volume
Serial Number : 0xd0b634d9
Max Component Length : 255
File System Name : NTFS
Supports Named Streams
Is DAX Volume
```

若要查詢 NTFS 特定磁片區資訊的磁片磁碟機 F，請輸入：

```
fsutil fsinfo ntfsinfo f:
```

輸出類似下列顯示：

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors : 0x00000000010ea04f
Total Clusters : 0x000000000021d409
Mft Zone End : 0x0000000000004700
```

若要查詢檔案系統的基礎硬體以取得磁區資訊，請輸入：

```
fsinfo sectorinfo d:
```

輸出類似下列顯示：

```
D:\>fsutil fsinfo sectorinfo d:
LogicalBytesPerSector : 4096
PhysicalBytesPerSectorForAtomicity : 4096
Trim Not Supported
DAX capable
```

若要查詢磁片磁碟機 E 的檔案系統統計資料，請輸入：

```
fsinfo statistics e:
```

輸出類似下列顯示：

```
File System Type : NTFS
Version : 1
UserFileReads : 75021
UserFileReadBytes : 1305244512
LogFileWriteBytes : 180936704
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
