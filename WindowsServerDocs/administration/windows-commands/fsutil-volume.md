---
title: fsutil volume
description: Fsutil volume 命令的參考檔，它會卸載磁片區，或查詢硬碟以判斷硬碟目前有多少可用空間，或哪個檔案正在使用特定叢集。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: c51d7c993199c395d2074fc1db393239a9b4b603
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032886"
---
# <a name="fsutil-volume"></a>fsutil volume

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

卸下磁片區，或查詢硬碟以判斷硬碟目前有多少可用空間，或哪個檔案正在使用特定叢集。

## <a name="syntax"></a>語法

```
fsutil volume [allocationreport] <volumepath>
fsutil volume [diskfree] <volumepath>
fsutil volume [dismount] <volumepath>
fsutil volume [filelayout] <volumepath> <fileID>
fsutil volume [list]
fsutil volume [querycluster] <volumepath> <cluster> [<cluster>] … …
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| allocationreport | 顯示如何在指定的磁片區上使用儲存體的相關資訊。 |
| `<volumepath>` | 指定磁碟機號 (後面加上冒號) 。 |
| diskfree | 查詢硬碟以判斷磁片磁碟機上的可用空間量。 |
| 卸載 - dismount | 卸載磁片區。 |
| filelayout | 顯示指定檔案的 NTFS 中繼資料。 |
| `<fileID>` | 指定檔案識別碼。 |
| list | 列出系統上的所有磁片區。 |
| querycluster | 找出哪個檔案正在使用指定的叢集。 您可以使用 **querycluster** 參數指定多個叢集。 |
| `<cluster>` | 指定 (LCN) 的邏輯群集編號。 |

### <a name="examples"></a>範例

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

若要顯示指定之檔案的所有相關資訊 (s) ，請輸入：

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

若要列出磁片上的磁片區，請輸入：

```
fsutil volume list
```

若要尋找在磁片磁碟機 C 上使用叢集（由邏輯叢集編號50和0x2000 指定）的 (s) ，請在磁片磁碟機 C 上輸入：

```
fsutil volume querycluster C: 50 0x2000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [NTFS 的運作方式](/previous-versions/windows/it-pro/windows-server-2003/cc781134(v=ws.10))
