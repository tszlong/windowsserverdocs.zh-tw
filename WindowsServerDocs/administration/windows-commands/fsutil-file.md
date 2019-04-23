---
ms.assetid: 9f3dc104-dd69-4b03-b824-a29896780164
title: Fsutil 檔案
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: ffaf02f74f20f4eb94b94d8f0ffc51f26a62390e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828119"
---
# <a name="fsutil-file"></a>Fsutil 檔案
>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

（如果已啟用磁碟配額），依使用者名稱尋找檔案、 查詢的檔案配置的範圍、 設定檔案的簡短名稱、 設定檔案的有效的資料長度、 設定零個資料檔案，或建立新的檔案。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil file [createnew] <filename> <length>
fsutil file [findbysid] <username> <directory>
fsutil file [optimizemetadata] [/A] <filename>
fsutil file [queryallocranges] offset=<offset> length=<length> <filename>
fsutil file [queryextents] [/R] <filename> [<startingvcn> [<numvcns>]]
fsutil file [queryfileid] <filename>
fsutil file [queryfilenamebyid] <volume> <fileid>
fsutil file [queryoptimizemetadata] <filename>
fsutil file [queryvaliddata] [/R] [/D] <filename>
fsutil file [seteof] <filename> <length>
fsutil file [setshortname] <filename> <shortname>
fsutil file [setvaliddata] <filename> <datalength>
fsutil file [setzerodata] offset=<offset> length=<length> <filename>

```

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|createnew|建立指定的名稱和大小的檔案，與由零組成的內容。|
|\<filename>|指定包含檔案名稱和副檔名，例如 C:\documents\filename.txt 檔案的完整路徑。|
|\<length>|指定檔案的有效的資料長度。|
|findbysid|會尋找屬於指定的使用者，NTFS 磁碟區上啟用磁碟配額的位置的檔案。|
|\<username>|指定使用者的使用者名稱或登入名稱。|
|\<directory>|指定的目錄，例如 C:\users 的完整路徑。|
|optimizemetadata|這會執行指定的檔案中繼資料的立即壓縮。|
|/A|最佳化之前和之後，請分析檔案中繼資料。|
|queryallocranges|查詢的 NTFS 磁碟區上的檔案已配置的範圍。 適用於判斷檔案是否疏鬆區域。|
|offset=\<offset>|指定應該設定為零的範圍開頭。|
|length=\<length>|指定的長度 （以位元組為單位） 的範圍。|
|queryextents|查詢範圍的檔案。|
|/R|如果<filename>是重新分析點，請開啟它，而非其目標。|
|\<startingvcn>|指定要查詢的第一個 VCN。 如果省略，開始 VCN 0。|
|\<numvcns>|若要查詢的 VCNs 數目。 如果省略則為 0，直到 EOF 的查詢。|
|queryfileid|查詢的檔案在 NTFS 磁碟區上的檔案識別碼。<br /><br />此參數適用於：Windows Server 2008 R2 和 Windows 7。|
|\<volume>|指定的磁碟區做為磁碟機名稱，後面接著冒號。|
|queryfilenamebyid|NTFS 磁碟區，會顯示指定的檔案識別碼的隨機的連結名稱。 由於檔案可以有一個以上的連結名稱，指向該檔案，不保證哪一個檔案的連結會提供查詢的結果檔案名稱。<br /><br />此參數適用於：Windows Server 2008 R2 和 Windows 7。|
|\<fileid>|指定 NTFS 磁碟區上的檔案的識別碼。|
|queryoptimizemetadata|查詢中繼資料檔案的狀態。|
|queryvaliddata|查詢檔案的有效的資料長度。|
|/D|顯示詳細的有效資料的資訊。|
|seteof|設定指定檔案的 EOF。|
|setshortname|設定 NTFS 磁碟區上的檔案的簡短名稱 （8.3 字元長度的檔案名稱）。|
|\<shortname>|指定檔案的簡短名稱。|
|setvaliddata|設定有效的資料長度的 NTFS 磁碟區上的檔案。|
|\<datalength>|指定檔案的長度，以位元組為單位。|
|setzerodata|將範圍設定 (所指定*位移*並*長度*) 為零的檔案，這會清空檔案。 如果檔案是疏鬆檔案，將基礎的配置單位是已取消認可。|

## <a name="remarks"></a>備註

-   NTFS，在中，有的檔案長度的兩個重要概念： 的檔案結尾 (EOF) 標記，並在有效的資料長度 (VDL)。 EOF 指出檔案的實際長度。 VDL 識別磁碟上的有效資料的長度。 自動 VDL 與 EOF 之間的任何讀取會傳回 0，以保留 C2 物件重複使用的需求。

-   **Setvaliddata**參數僅適用於系統管理員，因為它需要執行磁碟區維護工作 (SeManageVolumePrivilege) 權限。 這項功能才需要進階多媒體與系統區域網路案例。 **Setvaliddata**參數必須是大於目前的 VDL，但目前的檔案大小大於或等於的正值。

    它可用於設定 VDL 程式時：

    -   寫入未經處理的叢集，直接以透過硬體通道磁碟。 這可讓程式，以通知這個範圍中包含有效的資料可以傳回給使用者的檔案系統。

    -   效能問題時，請建立大型檔案。 這可避免填滿零的檔案，建立或擴充檔時所花費的時間。

## <a name="BKMK_examples"></a>範例
若要尋找 scottb C 磁碟機上所擁有的檔案，請輸入：

```
fsutil file findbysid scottb c:\users  
```

若要查詢的 NTFS 磁碟區上的檔案已配置的範圍，請輸入：

```
fsutil file queryallocranges offset=1024 length=64 c:\temp\sample.txt  
```

若要最佳化的檔案中繼資料，請輸入：

```
fsutil file optimizemetadata C:\largefragmentedfile.txt
```

若要查詢的檔案範圍，請輸入：

```
fsutil file queryextents C:\Temp\sample.txt
```

若要設定檔案 EOF，請輸入：

```
fsutil file seteof C:\testfile.txt 1000
```

若要設定磁碟機 C Longfile.txt 檔案 Longfilename.txt 的簡短名稱，請輸入：

```
fsutil file setshortname c:\longfilename.txt longfile.txt  
```

若要設定有效的資料長度為 4096 個位元組的 NTFS 磁碟區上名為 Testfile.txt 的檔案，請輸入：

```
fsutil file setvaliddata c:\testfile.txt 4096  
```

若要將範圍設定為 NTFS 磁碟區上的檔案為清空它為零，輸入：

```
fsutil file setzerodata offset=100 length=150 c:\temp\sample.txt  
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


