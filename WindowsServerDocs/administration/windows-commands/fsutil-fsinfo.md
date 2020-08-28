---
title: Fsutil fsinfo
description: Fsutil fsinfo 命令的參考文章，它會列出所有磁片磁碟機、查詢磁片磁碟機類型、查詢磁片區資訊、查詢 NTFS 特定的磁片區資訊，或查詢檔案系統統計資料。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: e921b8572b7d1d87a1baf40cfdbc955adce3ec01
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037366"
---
# <a name="fsutil-fsinfo"></a>fsutil fsinfo

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

列出所有磁片磁碟機、查詢磁片磁碟機類型、查詢磁片區資訊、查詢 NTFS 特定的磁片區資訊，或查詢檔案系統統計資料。

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
| ntfsinfo | 列出指定磁片區的 NTFS 特定磁片區資訊，例如磁區數目、總叢集數、可用叢集，以及 MFT 區域的開頭和結尾。 |
| sectorinfo | 列出硬體磁區大小和對齊方式的相關資訊。 |
| 統計資料 | 列出指定磁片區的檔案系統統計資料，例如中繼資料、記錄檔以及 MFT 讀取和寫入。 |
| volumeinfo | 列出指定磁片區的資訊，例如檔案系統，以及磁片區是否支援區分大小寫的檔案名、檔案名中的 unicode、磁片配額，或是 (DAX) 磁片區的 DirectAccess。 |
| `<volumepath>:` | 指定磁碟機號 (後面加上冒號) 。 |
| `<rootpath>:` | 指定磁碟機號 (後面接著根磁片磁碟機的冒號) 。 |

### <a name="examples"></a>範例

若要列出電腦中的所有磁片磁碟機，請輸入：

```
fsutil fsinfo drives
```

如下所示的輸出：

```
Drives: A:\ C:\ D:\ E:\
```

若要查詢磁片磁碟機 C 的磁片磁碟機類型，請輸入：

```
fsutil fsinfo drivetype c:
```

查詢可能的結果包括：

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

如下所示的輸出：

```
Volume Name : Volume
Serial Number : 0xd0b634d9
Max Component Length : 255
File System Name : NTFS
Supports Named Streams
Is DAX Volume
```

若要針對 NTFS 特定磁片區資訊來查詢磁片磁碟機 F，請輸入：

```
fsutil fsinfo ntfsinfo f:
```

如下所示的輸出：

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors : 0x00000000010ea04f
Total Clusters : 0x000000000021d409
Mft Zone End : 0x0000000000004700
```

若要查詢檔案系統的基礎硬體中的磁區資訊，請輸入：

```
fsinfo sectorinfo d:
```

如下所示的輸出：

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

如下所示的輸出：

```
File System Type : NTFS
Version : 1
UserFileReads : 75021
UserFileReadBytes : 1305244512
LogFileWriteBytes : 180936704
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
