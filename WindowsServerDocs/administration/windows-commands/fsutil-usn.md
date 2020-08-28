---
title: fsutil usn
description: Fsutil usn 命令的參考文章，可管理更新序號 (USN) 變更日誌。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 551324f3a356d7ca3ebce617853000baee3c68e0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032926"
---
# <a name="fsutil-usn"></a>fsutil usn

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

管理更新序號 (USN) 變更日誌。 USN 變更日誌會提供對磁片區上檔案進行之所有變更的持續記錄。 新增、刪除和修改檔案、目錄和其他 NTFS 物件時，NTFS 會在 USN 變更日誌中輸入記錄，電腦上的每個磁片區都有一個記錄。 每個記錄都會指出變更的類型與變更的物件。 新記錄會附加到資料流程的結尾。

## <a name="syntax"></a>語法

```
fsutil usn [createjournal] m=<maxsize> a=<allocationdelta> <volumepath>
fsutil usn [deletejournal] {/d | /n} <volumepath>
fsutil usn [enablerangetracking] <volumepath> [options]
fsutil usn [enumdata] <fileref> <lowUSN> <highUSN> <volumepath>
fsutil usn [queryjournal] <volumepath>
fsutil usn [readdata] <filename>
fsutil usn [readjournal] [c= <chunk-size> s=<file-size-threshold>] <volumepath>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| createjournal | 建立 USN 變更日誌。 |
| m =`<maxsize>` | 指定 NTFS 為變更日誌配置的大小上限（以位元組為單位）。 |
| a =`<allocationdelta>` | 指定已新增至結尾並從變更日誌開頭移除的記憶體配置大小（以位元組為單位）。 |
| `<volumepath>` | 指定磁碟機號 (後面加上冒號) 。 |
| deletejournal | 刪除或停用 active USN 變更日誌。<p>**注意：** 刪除變更日誌會影響 (FRS) 和索引服務的檔案複寫服務，因為它需要這些服務來執行磁片區的完整 (和耗時的) 掃描。 在重新掃描磁片區時，這會對 FRS SYSVOL 複寫和 DFS 連結替代之間的複寫造成負面影響。 |
| /d | 停用使用中的 USN 變更日誌，並在變更日誌停用時，傳回 (i/o) 控制項的輸入/輸出。 |
| /n | 停用作用中的 USN 變更日誌，並只在停用變更日誌之後傳回 i/o 控制項。 |
| enablerangetracking | 啟用磁片區的 USN 寫入範圍追蹤。 |
| c =`<chunk-size>` | 指定要在磁片區上追蹤的區塊大小。 |
| s =`<file-size-threshold>` | 指定範圍追蹤的檔案大小臨界值。 |
| enumdata | 列舉並列出兩個指定界限之間的變更日誌專案。 |
| `<fileref>` | 指定要開始列舉之磁片區上檔案內的序數位置。 |
| `<lowUSN>` | 指定用來篩選所傳回記錄之 USN 值範圍的下限。 只會傳回最後變更日誌 USN 介於或等於 *lowUSN* 和 *highUSN* 成員值的記錄。 |
| `<highUSN>` | 指定用來篩選所傳回檔案之 USN 值範圍的上限。 |
| queryjournal | 查詢磁片區的 USN 資料，以收集目前變更日誌、其記錄及其容量的相關資訊。 |
| readdata | 讀取檔案的 USN 資料。 |
| `<filename>` | 指定檔案的完整路徑，包括檔案名和副檔名，例如： *C:\documents\filename.txt*。 |
| readjournal | 讀取 USN 日誌中的 USN 記錄。 |
| minver =`<number>` | 要傳回 USN_RECORD 的最小主要版本。 預設值 = 2。 |
| maxver =`<number>` | 要傳回 USN_RECORD 的最大主要版本。 預設值 = 4。 |
| startusn =`<USN number>` | 開始讀取 USN 日誌的 USN。 預設值 = 0。 |

#### <a name="remarks"></a>備註

- 程式可以參考 USN 變更日誌，判斷對一組檔案進行的所有修改。 USN 變更日誌比檢查時間戳記或註冊檔案通知更有效率。 USN 變更日誌已由索引服務啟用和使用、檔案複寫服務 (FRS) 、遠端安裝服務 (RIS) 和遠端存放。

- 如果磁片區上已經有變更日誌， **createjournal** 參數會更新變更日誌的 **maxsize** 和 **allocationdelta** 參數。 這可讓您展開使用中日誌維護的記錄數目，而不需要將它停用。

- 變更日誌的成長可能大於這個目標值，但在下一個 NTFS 檢查點會截斷變更日誌，使其小於此值。 NTFS 會檢查變更日誌，並在其大小超過 **maxsize** 的值加上 **allocationdelta**的值時加以修剪。 在 NTFS 檢查點，作業系統會將記錄寫入 NTFS 記錄檔，該檔案可讓 NTFS 判斷從失敗復原所需的處理。

- 在修剪之前，變更日誌可以成長至超過 **maxsize** 和 **allocationdelta** 的值總和。

- 刪除或停用作用中的變更日誌非常耗時，因為系統必須存取 master 檔案資料表中的所有記錄 (MFT) ，並將最後一個 USN 屬性設定為 0 (零) 。 此程式可能需要幾分鐘的時間，如果需要重新開機，則可在系統重新開機之後繼續進行。 在此過程中，變更日誌不會被視為使用中，也不會停用。 當系統停用日誌時，無法存取，而且所有日誌作業都會傳回錯誤。 停用使用中的日誌時，您應該特別小心，因為這會對使用該日誌的其他應用程式造成負面影響。

### <a name="examples"></a>範例

若要在磁片磁碟機 C 上建立 USN 變更日誌，請輸入：

```
fsutil usn createjournal m=1000 a=100 c:
```

若要刪除磁片磁碟機 C 上的主動式 USN 變更日誌，請輸入：

```
fsutil usn deletejournal /d c:
```

若要使用指定的區塊大小和檔案大小-閾值來啟用範圍追蹤，請輸入：

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

若要在磁片磁碟機 C 的兩個指定界限之間列舉並列出變更日誌專案，請輸入：

```
fsutil usn enumdata 1 0 1 c:
```

若要查詢磁片磁碟機 C 上磁片區的 USN 資料，請輸入：

```
fsutil usn queryjournal c:
```

若要讀取磁片磁碟機 C 上 \Temp 資料夾中檔案的 USN 資料，請輸入：

```
fsutil usn readdata c:\temp\sample.txt
```

若要讀取具有特定開始 USN 的 USN 日誌，請輸入：

```
fsutil usn readjournal startusn=0xF00
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
