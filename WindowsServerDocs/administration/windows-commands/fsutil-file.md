---
title: fsutil file
description: Fsutil file 命令的參考主題，它會依使用者名稱尋找檔案、針對檔案配置查詢範圍、設定檔案的簡短名稱、設定檔案的有效資料長度、為檔案設定零個數據，或建立新的檔案。
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 9f3dc104-dd69-4b03-b824-a29896780164
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: e9be8f6d21b89d1017371b9697e1227122826a7d
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435873"
---
# <a name="fsutil-file"></a>fsutil file

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

依使用者名稱尋找檔案（如果已啟用磁片配額）、設定檔案範圍的查詢、設定檔案的簡短名稱、設定檔案的有效資料長度、為檔案設定零個數據，或建立新的檔案。

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

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| createnew | 建立具有指定之名稱和大小的檔案，其內容包含零。 |
| `<length>` | 指定檔案的有效資料長度。 |
| findbysid | 在已啟用磁片配額的 NTFS 磁片區上，尋找屬於指定使用者的檔案。 |
| `<username>` | 指定使用者的使用者名稱或登入名稱。 |
| `<directory>` | 指定目錄的完整路徑，例如 C:\users。 |
| optimizemetadata | 這會針對指定的檔案執行中繼資料的立即壓縮。 |
| /a | 在優化之前和之後分析檔案中繼資料。 |
| queryallocranges | 查詢 NTFS 磁片區上的檔案已配置的範圍。 適用于判斷檔案是否有稀疏區域。 |
| offset =`<offset>` | 指定應設定為零之範圍的開頭。 |
| 長度 =`<length>` | 指定範圍的長度（以位元組為單位）。 |
| queryextents | 檔的查詢範圍。 |
| /r | 如果 <filename> 是重新分析點，請開啟它，而不是其目標。 |
| `<startingvcn>` | 指定要查詢的第一個 VCN。 如果省略，則從 VCN 0 開始。 |
| `<numvcns>` | 要查詢的 VCNs 數目。 如果省略或為0，則查詢直到 EOF 為止。 |
| queryfileid | 查詢 NTFS 磁片區上檔案的檔案識別碼。 |
| `<volume>` | 將磁片區指定為磁片磁碟機名稱，後面接著冒號。 |
| queryfilenamebyid | 在 NTFS 磁片區上顯示指定檔案識別碼的隨機連結名稱。 由於檔案可以有一個以上指向該檔案的連結名稱，因此不保證會提供檔案連結做為檔案名的查詢結果。 |
| `<fileid>` | 在 NTFS 磁片區上指定檔案的識別碼。 |
| queryoptimizemetadata | 查詢檔案的中繼資料狀態。 |
| queryvaliddata | 查詢檔案的有效資料長度。 |
| /d | 顯示詳細的有效資料資訊。 |
| seteof | 設定指定檔案的 EOF。 |
| setshortname | 為 NTFS 磁片區上的檔案設定簡短名稱（8.3 字元長度的檔案名）。 |
| `<shortname>` | 指定檔案的簡短名稱。 |
| setvaliddata | 為 NTFS 磁片區上的檔案設定有效的資料長度。 |
| `<datalength>` | 指定檔案的長度（以位元組為單位）。 |
| setzerodata | 將檔案的範圍（以*位移*和*長度*指定）設定為零，這會清空檔案。 如果檔案是稀疏檔案，則會已取消認可基礎配置單位。 |

#### <a name="remarks"></a>備註

- 在 NTFS 中，檔案長度有兩個重要的概念：檔案結尾（EOF）標記和有效的資料長度（VDL）。 EOF 表示檔案的實際長度。 VDL 會識別磁片上有效資料的長度。 VDL 和 EOF 之間的任何讀取都會自動傳回0，以保留 C2 物件重複使用需求。

- **Setvaliddata**參數僅適用于系統管理員，因為它需要「執行磁片區維護工作」（SeManageVolumePrivilege）許可權。 只有先進的多媒體和系統區域網路絡案例才需要這項功能。 **Setvaliddata**參數必須是大於目前 VDL 但小於目前檔案大小的正數值。

    這適用于在下列情況中設定 VDL 的程式：

    - 透過硬體通道將原始叢集直接寫入磁片。 這可讓程式通知檔案系統，此範圍包含可傳回給使用者的有效資料。

    - 當效能問題時，建立大型檔案。 這可避免在檔案建立或擴充時，以零填滿檔案所花費的時間。

### <a name="examples"></a>範例

若要尋找*scottb*在磁片磁碟機 C 上所擁有的檔案，請輸入：

```
fsutil file findbysid scottb c:\users
```

若要查詢 NTFS 磁片區上某個檔案的配置範圍，請輸入：

```
fsutil file queryallocranges offset=1024 length=64 c:\temp\sample.txt
```

若要優化檔案的中繼資料，請輸入：

```
fsutil file optimizemetadata C:\largefragmentedfile.txt
```

若要查詢檔案的範圍，請輸入：

```
fsutil file queryextents C:\Temp\sample.txt
```

若要設定檔案的 EOF，請輸入：

```
fsutil file seteof C:\testfile.txt 1000
```

若要將檔案的簡短名稱*longfilename* （磁片磁碟機 C）設定為*longfile*，請輸入：

```
fsutil file setshortname c:\longfilename.txt longfile.txt
```

若要針對 NTFS 磁片區上名為*testfile.txt*的檔案，將有效的資料長度設定為*4096 個位元組*，請輸入：

```
fsutil file setvaliddata c:\testfile.txt 4096
```

若要將 NTFS 磁片區上的某個檔案範圍設定為零，將它設為空白，請輸入：

```
fsutil file setzerodata offset=100 length=150 c:\temp\sample.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
