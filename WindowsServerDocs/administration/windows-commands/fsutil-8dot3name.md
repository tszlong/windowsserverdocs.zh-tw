---
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
title: Fsutil 8dot3name
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b0261daf230cbf93951a9c111fdbf5aafefb061b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377046"
---
# <a name="fsutil-8dot3name"></a>Fsutil 8dot3name

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

查詢或變更簡短名稱（8.3 名稱）行為的設定，其中包括：

-   查詢簡短名稱行為的目前設定

-   如果已從指定的目錄路徑移除簡短名稱，請掃描指定的登錄機碼目錄路徑

-   變更控制簡短名稱行為的設定。 此設定可套用至指定的磁片區或預設的磁片區設定。

-   移除目錄中所有檔案的簡短名稱

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil 8dot3name [query] [<VolumePath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <DirectoryPath>
fsutil 8dot3name [set] { <DefaultValue> | <VolumePath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <DirectoryPath>
```

## <a name="parameters"></a>參數

|                 參數                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           查詢 [<VolumePath>]            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           在檔案系統中查詢8.3 簡短名稱建立行為的狀態。<br /><br />如果未將*VolumePath*指定為參數，則會顯示所有磁片區的預設 [8dot3name 建立行為] 設定。                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|           掃描 <DirectoryPath>            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        掃描位於指定*DirectoryPath*中的登錄機碼檔案，這些檔案可能會在從檔案名中移除8.3 簡短名稱時受到影響。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 設定 {<DefaultValue> &#124; <VolumePath>} | 變更下列實例中，建立8.3 名稱的檔案系統行為：<br /><br /><ul><li>當指定*defaultvalue*時，登錄機碼**HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**會設定為*defaultvalue*.<br /><br />    *DefaultValue*可以具有下列值：<br /><br /><ul><li>**0**：為系統上的所有磁片區啟用8.3 名稱建立。</li><li>**1**：停用系統上所有磁片區的8dot3 名稱建立。</li><li>**2**：設定以每個磁片區為基礎的8dot3 名稱建立。</li><li>**3**：停用所有磁片區的8dot3 名稱建立，但不包括系統磁碟區。</li></ul></li><li>當指定*VolumePath*時，會將磁片旗標8dot3name 屬性上指定的磁片區設定為啟用指定磁片區的8dot3 名稱建立（**0**），或設定為停用指定磁片區上的8dot3 名稱建立（**1**）。<br /><br />    您必須先將8.3 名稱建立的預設檔案系統行為設定為值**2** ，才能啟用或停用指定磁片區的8dot3 名稱建立。</li></ul> |
|           帶狀 <DirectoryPath>           |                                                                                                                                                                                                                                                                                                                  移除位於指定*DirectoryPath*之所有檔案的8.3 檔案名。 若檔案的*DirectoryPath*與檔案名結合時包含超過260個字元，則不會移除該檔案名。<br /><br />此命令會列出，但不會修改指向永久移除8.3 檔案名之檔案的登錄機碼。<br /><br />如需從檔案永久移除8.3 檔案名的效果的詳細資訊，請參閱[備註](Fsutil-8dot3name.md#BKMK_remarks)。                                                                                                                                                                                                                                                                                                                  |
|               <VolumePath>                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       指定磁片磁碟機名稱，後面接著冒號或格式為**磁片區 {** <em>GUID</em> **}** 的 GUID。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|                    /f                     |                                                                                                                                                                                                                                                                                                   指定在指定*DirectoryPath*中的所有檔案都已移除8.3 檔案名，即使有使用8.3 檔案名指向檔案的登錄機碼也一樣。 在此情況下，作業會移除8.3 檔案名，但不會修改指向使用8.3 檔案名之檔案的任何登錄機碼。 **警告：** 建議您在使用 **/f**參數之前先備份您的目錄或磁片區，因為它可能會導致非預期的應用程式失敗，包括無法卸載程式。                                                                                                                                                                                                                                                                                                    |
|              /l [<log file>]              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                  指定寫入資訊的記錄檔。<br /><br />如果未指定 **/l**參數，則會將所有資訊寫入預設記錄檔：<br /><br />**%temp%\8dot3_removal_log @ （** <em>GMT YYYY-MM-DD HH-mm-SS</em> **） .log**                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                    /s                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      指定應該將作業套用至指定之*DirectoryPath*的子目錄。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|                    一起                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                          指定移除8.3 檔案名應該在測試模式中執行。 除了實際移除的8dot3 檔案名以外，所有的作業都會執行。 您可以使用測試模式來探索哪些登錄機碼指向使用8.3 檔案名的檔案。                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                    /v                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       指定寫入記錄檔的所有資訊也會顯示在命令列上。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="BKMK_remarks"></a>標記

-   永久移除8.3 檔案名，而不要修改指向8.3 檔案名的登錄機碼，可能會導致非預期的應用程式失敗，包括無法卸載應用程式。 建議您先備份您的目錄或磁片區，再嘗試移除8.3 檔案名。

## <a name="BKMK_examples"></a>典型
若要針對以 GUID {928842df-5a01-11de-a85c-806e6f6e6963} 指定的磁片區查詢停用的8dot3 名稱行為，請輸入：

```
fsutil 8dot3name query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

您也可以使用 [**行為**] 子命令來查詢8dot3 名稱行為。

若要在*D:\MyData*目錄和所有子目錄中移除8.3 檔案名，同時將資訊寫入指定為*mylogfile*的記錄檔，請輸入：

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fsutil 8dot3name](Fsutil-8dot3name.md)

[Fsutil 行為](Fsutil-behavior.md)


