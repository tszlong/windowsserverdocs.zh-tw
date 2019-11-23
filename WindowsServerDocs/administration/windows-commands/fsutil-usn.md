---
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
title: Fsutil usn
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b62d031c547f140ac5008af20a9e0ee4bcecc919
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376794"
---
# <a name="fsutil-usn"></a>Fsutil usn
>適用于： Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

管理更新序號（USN）變更日誌。

## <a name="syntax"></a>語法

```
fsutil usn [createjournal] m=<MaxSize> a=<AllocationDelta> <VolumePath>
fsutil usn [deletejournal] {/D | /N} <volumepath>
fsutil usn [enablerangetracking] <volumepath> [options]
fsutil usn [enumdata] <FileRef> <LowUSN> <HighUSN> <VolumePath>
fsutil usn [queryjournal] <VolumePath>
fsutil usn [readdata] <FileName>
fsutil usn [readjournal] [c= <chunk-size> s=<file-size-threshold>] <volumepath>
```

## <a name="parameters"></a>Parameters

|參數|描述|
|-------------|---------------|
|createjournal|建立 USN 變更日誌。|
|m =\<MaxSize >|指定 NTFS 為變更日誌配置的大小上限（以位元組為單位）。|
|a =\<AllocationDelta >|指定新增至結尾並從變更日誌開頭移除的記憶體配置大小（以位元組為單位）。|
|\<VolumePath >|指定磁碟機號（後面接著冒號）。|
|deletejournal|刪除或停用 active USN 變更日誌。 **注意：** 刪除變更日誌會影響檔案複寫服務（FRS）和索引服務，因為它需要這些服務來執行磁片區的完整（且耗時）掃描。 這在重新掃描磁片區時，會反過來對 FRS SYSVOL 複寫和 DFS 連結替代之間的複寫造成負面影響。|
|/d|停用 active USN 變更日誌，並在停用變更日誌時傳回輸入/輸出（i/o）控制。|
|/n|停用使用中的 USN 變更日誌，只有在停用變更日誌之後，才會傳回 i/o 控制。|
|enablerangetracking|啟用磁片區的 USN 寫入範圍追蹤。|
|c =\<區塊大小 >|指定要在磁片區上追蹤的區塊大小。|
|s =\<檔案大小-閾值 >|指定範圍追蹤的檔案大小閾值。|
|enumdata|列舉並列出兩個指定界限之間的變更日誌專案。|
|\<FileRef >|指定要開始列舉之磁片區上檔案內的序數位置。|
|\<LowUSN >|指定用來篩選所傳回記錄之 USN 值範圍的下限。 只會傳回上次變更日誌 USN 介於或等於*LowUSN*和*HighUSN*成員值的記錄。|
|\<HighUSN >|指定用來篩選所傳回檔案之 USN 值範圍的上限。|
|queryjournal|查詢磁片區的 USN 資料，以收集目前變更日誌、其記錄及其容量的相關資訊。|
|readdata|讀取檔案的 USN 資料。|
|\<FileName >|指定檔案的完整路徑，包括檔案名和副檔名，例如： C:\documents\filename.txt|
|readjournal|讀取 USN 日誌中的 USN 記錄。|
|minver =\<數位 >|要傳回 USN_RECORD 的最小主要版本。 預設值 = 2。|
|maxver =\<數位 >|要傳回之 USN_RECORD 的最大主要版本。 預設值 = 4。|
|startusn =\<USN 號碼 >|要開始讀取 USN 日誌的 USN。 預設值 = 0。|


## <a name="remarks"></a>備註

-   關於 USN 變更日誌

    USN 變更日誌會針對磁片區上的檔案所做的所有變更，提供持續性記錄。 新增、刪除和修改檔案、目錄和其他 NTFS 物件時，NTFS 會將記錄輸入到 USN 變更日誌中，這是電腦上的每個磁片區。 每個記錄都會指出變更的類型與變更的物件。 新的記錄會附加到資料流程的結尾。

    程式可以查閱 USN 變更日誌，以判斷對一組檔案所做的所有修改。 USN 變更日誌比檢查時間戳記或註冊檔案通知更有效率。 USN 變更日誌是由索引服務、檔案複寫服務（FRS）、遠端安裝服務（RIS）和遠端存放啟用及使用。

-   使用**createjournal**

    如果磁片區上已有變更日誌， **createjournal**參數會更新變更日誌的*MaxSize*和*AllocationDelta*參數。 這可讓您擴充使用中日誌維護的記錄數目，而不需要將它停用。

-   使用**m**

    變更日誌可能會成長到大於此目標值，但變更日誌會在下一個 NTFS 檢查點截斷到小於此值。 NTFS 會檢查變更日誌，並在其大小超過*MaxSize*的值加上*AllocationDelta*的值時加以修剪。 在 NTFS 檢查點，作業系統會將記錄寫入 NTFS 記錄檔，讓 NTFS 能夠判斷從失敗復原所需的處理。

-   使用

    在修剪之前，變更日誌可以成長到大於*MaxSize*和*AllocationDelta*值的總和。

-   使用**deletejournal**

    刪除或停用使用中的變更日誌非常耗時，因為系統必須存取主要檔案資料表（MFT）中的所有記錄，並將最後一個 USN 屬性設為0（零）。 此程式可能需要幾分鐘的時間，而且如果需要重新開機，它可以在系統重新開機之後繼續進行。 在此過程中，變更日誌不會被視為作用中，也不會停用。 當系統停用日誌時，就無法存取它，而且所有日誌作業都會傳回錯誤。 停用使用中的日誌時，您應該特別小心，因為它會對其他使用日誌的應用程式造成不良影響。

## <a name="BKMK_examples"></a>典型
若要在 C 磁片磁碟機上建立 USN 變更日誌，請輸入：

```
fsutil usn createjournal m=1000 a=100 c:
```

若要刪除磁片磁碟機 C 上的 active USN 變更日誌，請輸入：

```
fsutil usn deletejournal /d c:
```

若要使用指定的區塊大小和檔案大小閾值來啟用範圍追蹤，請輸入：

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

若要列舉並列出磁片磁碟機 C 上兩個指定界限之間的變更日誌專案，請輸入：

```
fsutil usn enumdata 1 0 1 c:
```

若要查詢磁片磁碟機 C 上磁片區的 USN 資料，請輸入：

```
fsutil usn queryjournal c:
```

若要讀取磁片磁碟機 C 上 [\Temp] 資料夾中檔案的 USN 資料，請輸入：

```
fsutil usn readdata c:\temp\sample.txt
```

若要讀取具有特定開始 USN 的 USN 日誌，請輸入：

```
fsutil usn readjournal startusn=0xF00
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


