---
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
title: fsutil usn
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4bef63f4938b5ce786bbbfdbf3bdd2081280ce95
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829979"
---
# <a name="fsutil-usn"></a>fsutil usn
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

管理更新序列號碼 (USN) 變更日誌。

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

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|createjournal|建立 USN 變更日誌。|
|m=\<MaxSize>|指定的最大的大小，以位元組為單位，NTFS 配置供變更日誌。|
|a=\<AllocationDelta>|指定的大小，以位元組為單位的結尾加入和移除的起點變更日誌的記憶體配置。|
|\<VolumePath>|指定的磁碟機代號 （後面接著冒號）。|
|deletejournal|刪除或停用使用中的 USN 變更日誌。 **注意**：刪除變更日誌會影響檔案複寫服務 (FRS) 」 和 「 索引服務中，因為它會需要這些服務以執行完整的 （且耗時的） 掃描的磁碟區。 這會接著產生負面影響的 FRS SYSVOL 複寫和 DFS 連結替代項目時要重新掃描磁碟區之間的複寫。|
|/d|停用使用中的 USN 變更日誌，並會傳回輸入/輸出 (I/O) 控制項，而被停用變更日誌。|
|/n|停用使用中的 USN 變更日誌，並在變更日誌已停用之後，才會傳回 I/O 控制。|
|enablerangetracking|啟用追蹤磁碟區的 USN 寫入範圍。|
|c=\<chunk-size>|指定追蹤磁碟區上的區塊大小。|
|s=\<file-size-threshold>|指定追蹤範圍的檔案大小臨界值。|
|enumdata|列舉，並列出兩個指定的界限之間的變更日誌項目。|
|\<FileRef>|指定列舉型別是要開始在磁碟區上的檔案中的序數位置。|
|\<LowUSN>|指定用來篩選所傳回的記錄範圍的 USN 值的下限。 只有其上次變更 USN 日誌記錄是之間，或等於*LowUSN*並*HighUSN*會傳回成員的值。|
|\<HighUSN>|指定用來篩選所傳回的檔案之範圍的 USN 值上限。|
|queryjournal|查詢磁碟區的 USN 資料，以收集目前的變更日誌，其記錄，其容量的相關資訊。|
|readdata|讀取檔案的 USN 資料。|
|\<FileName>|指定的檔案，例如包括檔名和副檔名的完整路徑：C:\documents\filename.txt|
|readjournal|讀取的 USN 記錄 USN 日誌中。|
|minver=\<number>|USN_RECORD 傳回最小值的主要版本。 預設值 = 2。|
|maxver=\<number>|USN_RECORD 傳回最大值的主要版本。 預設值 = 4。|
|startusn =\<USN 數字 >|若要開始讀取從 USN 日誌的 USN。 預設值 = 0。|


## <a name="remarks"></a>備註

-   關於 USN 變更日誌

    USN 變更日誌提供磁碟區上的檔案所做的所有變更的持續記錄。 加入、 刪除和修改檔案、 目錄和其他 NTFS 物件時，NTFS 會進入 USN 變更日誌，其中每個磁碟區的電腦上的記錄。 每個記錄都會指出變更的類型與變更的物件。 新的記錄會附加至資料流的結尾。

    程式可以查閱 USN 變更日誌，來判斷對一組檔案的所有修改。 USN 變更日誌會比檢查時間戳記，或註冊檔案通知更有效率。 啟用並編製索引的服務、 檔案複寫服務 (FRS)、 遠端安裝服務 (RIS) 和遠端存放裝置使用 USN 變更日誌。

-   使用**createjournal**

    如果磁碟區上已有變更日誌**createjournal**參數會更新變更日誌*MaxSize*並*AllocationDelta*參數。 這可讓您擴充的使用中的日誌維護，而不必將它停用的記錄數目。

-   使用**m**

    變更日誌可以成長到大於此目標值，但變更日誌會被截斷為下一個 NTFS 檢查點，到小於此值。 NTFS 會檢查變更日誌，並當其大小超過的值會修剪*MaxSize*的值加*AllocationDelta*。 在 NTFS 的檢查點，作業系統會將記錄寫入至 NTFS 記錄檔可讓 NTFS，才能判斷哪些處理，才能從失敗中復原。

-   使用

    變更日誌所能成長到多個值的總和*MaxSize*並*AllocationDelta*之前要修剪。

-   使用**deletejournal**

    刪除或停用作用中的變更日誌是很耗費時間，因為系統必須存取主檔案表格 (MFT) 中的所有記錄，並將最後一個 USN 屬性設為 0 （零）。 此程序可能需要幾分鐘的時間，並需要重新啟動時，它可以繼續在系統重新啟動之後。 在此過程中，變更日誌不是作用中，也不已經停用。 系統停用日誌時，無法存取，而所有的 「 日誌 」 作業會傳回錯誤。 您應該非常小心時停用使用中的日誌，因為它有不利的影響其他應用程式使用日誌。

## <a name="BKMK_examples"></a>範例
若要建立的 USN 變更日誌磁碟機 C，型別上：

```
fsutil usn createjournal m=1000 a=100 c:
```

若要刪除使用中 USN 變更日誌磁碟機 C，型別上：

```
fsutil usn deletejournal /d c:
```

若要啟用追蹤與指定的區塊大小和檔案大小臨界值的範圍，請輸入：

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

若要列舉，並列出 C 磁碟機上的兩個指定界限之間的變更日誌項目，請輸入：

```
fsutil usn enumdata 1 0 1 c:
```

若要查詢的 C 磁碟機上的磁碟區的 USN 資料，請輸入：

```
fsutil usn queryjournal c:
```

若要閱讀 \Temp 資料夾中的檔案，在 C 磁碟機上的 USN 資料，請輸入：

```
fsutil usn readdata c:\temp\sample.txt
```

若要讀取特定開始 USN 與 USN 日誌，請輸入：

```
fsutil usn readjournal startusn=0xF00
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


