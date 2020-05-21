---
title: fsutil 8dot3name
description: Fsutil 8dot3name 命令的參考主題，其會查詢或變更簡短名稱（8.3 名稱）行為的設定。
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 02977a33c21560fd2078f0f596f312f4ab4bc408
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436053"
---
# <a name="fsutil-8dot3name"></a>fsutil 8dot3name

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

查詢或變更簡短名稱（8.3 名稱）行為的設定，其中包括：

- 查詢簡短名稱行為的目前設定。

- 如果已從指定的目錄路徑移除簡短名稱，則掃描指定的登錄機碼目錄路徑。

- 變更控制簡短名稱行為的設定。 此設定可套用至指定的磁片區或預設的磁片區設定。

- 移除目錄中所有檔案的簡短名稱。

> [!IMPORTANT]
> 永久移除8.3 檔案名，而不要修改指向8.3 檔案名的登錄機碼，可能會導致非預期的應用程式失敗，包括無法卸載應用程式。 建議您先備份您的目錄或磁片區，再嘗試移除8.3 檔案名。

## <a name="syntax"></a>語法

```
fsutil 8dot3name [query] [<volumepath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <directorypath>
fsutil 8dot3name [set] { <defaultvalue> | <volumepath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <directorypath>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 查詢`[<volumepath>]` | 在檔案系統中查詢8.3 簡短名稱建立行為的狀態。<p>如果未將*volumepath*指定為參數，則會顯示所有磁片區的預設 [8dot3name 建立行為] 設定。 |
| 搜索`<directorypath>` | 掃描位於指定*directorypath*中的登錄機碼檔案，這些檔案可能會在從檔案名中移除8.3 簡短名稱時受到影響。 |
| 設定`<defaultvalue> | <volumepath>}` | 變更下列實例中，建立8.3 名稱的檔案系統行為：<ul><li>當指定*defaultvalue*時，登錄機碼**HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**會設定為*defaultvalue*。<p>*DefaultValue*可以具有下列值：<ul><li>**0**：為系統上的所有磁片區啟用8.3 名稱建立。</li><li>**1**：停用系統上所有磁片區的8dot3 名稱建立。</li><li>**2**：設定以每個磁片區為基礎的8dot3 名稱建立。</li><li>**3**：停用所有磁片區的8dot3 名稱建立，但不包括系統磁碟區。</li></ul><li>當指定*volumepath*時，會將磁片旗標8dot3name 屬性上指定的磁片區設定為啟用指定磁片區的8dot3 名稱建立（**0**），或設定為停用指定磁片區上的8dot3 名稱建立（**1**）。<p>您必須先將8.3 名稱建立的預設檔案系統行為設定為值**2** ，才能啟用或停用指定磁片區的8dot3 名稱建立。</li></ul> |
| 條紋`<directorypath>` | 移除位於指定*directorypath*之所有檔案的8.3 檔案名。 若檔案的*directorypath*與檔案名結合時包含超過260個字元，則不會移除該檔案名。<p>此命令會列出，但不會修改指向永久移除8.3 檔案名之檔案的登錄機碼。 |
| `<volumepath>` | 指定磁片磁碟機名稱，後面接著冒號或 GUID，格式為 `volume{GUID}` 。 |
| /f | 指定在指定*directorypath*中的所有檔案都已移除8.3 檔案名，即使有使用8.3 檔案名指向檔案的登錄機碼也一樣。 在此情況下，作業會移除8.3 檔案名，但不會修改指向使用8.3 檔案名之檔案的任何登錄機碼。 **警告：** 建議您在使用 **/f**參數之前先備份您的目錄或磁片區，因為它可能會導致非預期的應用程式失敗，包括無法卸載程式。 |
| /l`[<log file>]` | 指定寫入資訊的記錄檔。<p>如果未指定 **/l**參數，則會將所有資訊寫入預設記錄檔中： `%temp%\8dot3_removal_log@(GMT YYYY-MM-DD HH-MM-SS)` .log * * |
| /s | 指定應該將作業套用至指定之*directorypath*的子目錄。 |
| /t | 指定移除8.3 檔案名應該在測試模式中執行。 除了實際移除的8dot3 檔案名以外，所有的作業都會執行。 您可以使用測試模式來探索哪些登錄機碼指向使用8.3 檔案名的檔案。 |
| /v | 指定寫入記錄檔的所有資訊也會顯示在命令列上。 |

### <a name="examples"></a>範例

若要針對以 GUID {928842df-5a01-11de-a85c-806e6f6e6963} 指定的磁片區查詢停用的8dot3 名稱行為，請輸入：

```
fsutil 8dot3name query volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

您也可以使用 [**行為**] 子命令來查詢8dot3 名稱行為。

若要在*D:\MyData*目錄和所有子目錄中移除8.3 檔案名，同時將資訊寫入指定為*mylogfile*的記錄檔，請輸入：

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [fsutil behavior](fsutil-behavior.md)
