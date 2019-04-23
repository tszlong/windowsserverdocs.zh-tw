---
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
title: Fsutil 8dot3name
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4e8d67de42342f5a0828df9f57ca6d7e2870586e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873189"
---
# <a name="fsutil-8dot3name"></a>Fsutil 8dot3name

>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

查詢或變更的設定簡短名稱 （8.3 名稱） 的行為，包括：

-   查詢的簡短名稱行為的目前設定

-   掃描指定的目錄路徑，如果從指定的目錄路徑已移除的簡短名稱可能會受影響的登錄機碼

-   變更控制的簡短名稱行為的設定。 這項設定可以套用至指定的磁碟區或預設的磁碟區設定。

-   移除目錄內的所有檔案的簡短名稱

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil 8dot3name [query] [<VolumePath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <DirectoryPath>
fsutil 8dot3name [set] { <DefaultValue> | <VolumePath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <DirectoryPath>
```

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|query [<VolumePath>]|查詢檔案系統狀態的 8.3 簡短名稱建立行為。<br /><br />如果*VolumePath*不指定做為參數，預設 8dot3name （英文） 建立行為設定，針對所有磁碟區會顯示。|
|掃描 <DirectoryPath>|掃描的檔案，位於所指定*DirectoryPath*如果 8.3 短檔名所移除的檔案名稱可能會受影響的登錄機碼。|
|set { <DefaultValue> &#124; <VolumePath>}|變更檔案系統的行為會在下列情況的 8.3 名稱建立：<br /><br /><ul><li>當*DefaultValue*指定時，登錄機碼**HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**，設定為*DefaultValue*。<br /><br />    *DefaultValue*可以有下列值：<br /><br /><ul><li>**0**:啟用在系統上的所有磁碟區的 8.3 名稱建立的功能。</li><li>**1**:停用系統上的所有磁碟區的 8.3 名稱建立。</li><li>**2**:設定以每個磁碟區上的 8.3 名稱建立。</li><li>**3**:停用系統磁碟區以外的所有磁碟區的 8.3 名稱建立。</li></ul></li><li>當*VolumePath*指定，則指定的磁碟區上磁碟旗標 8dot3name （英文） 屬性會設定為啟用指定的磁碟區的 8.3 名稱建立 (**0**)，或設為停用上的 8.3 名稱建立指定磁碟區 (**1**)。<br /><br />    您必須設定為值 8.3 名稱建立的預設檔案系統行為**2**您可以啟用或停用指定的磁碟區的 8.3 名稱建立之前。</li></ul>|
|寬帶 <DirectoryPath>|移除所有的檔案所在的 8.3 檔案名稱中指定*DirectoryPath*。 8.3 檔案名稱不會移除所有的檔案位置*DirectoryPath*結合與檔案名稱包含超過 260 個字元。<br /><br />此命令會列出，但不會修改登錄機碼指向已永久移除 8.3 檔案名稱的檔案。<br /><br />永久地從檔案移除 8.3 檔案名稱的效果的相關資訊，請參閱[備註](Fsutil-8dot3name.md#BKMK_remarks)。|
|<VolumePath>|指定磁碟機名稱，後面接著冒號或格式的 GUID**磁碟區 {***GUID***}**。|
|/f|指定的所有檔案，在指定*DirectoryPath* 8.3 檔案名稱移除了即使有指向檔案使用 8.3 檔案名稱的登錄機碼。 在此情況下，作業會移除 8.3 檔案名稱，但不會修改任何登錄機碼指向使用 8.3 檔案名稱的檔案。 **警告：** 建議您備份您的目錄或磁碟區，才能使用 **/f**參數因為它可能會導致非預期的應用程式失敗，包括無法解除安裝程式。|
|/l [<log file>]|指定記錄檔寫入資訊的位置。<br /><br />如果 **/l**未指定參數，所有的資訊會寫入至預設的記錄檔：<br /><br />**%temp%\8dot3_removal_log@(***GMT YYYY-MM-DD HH-MM-SS***).log**|
|/s|指定的作業應該會套用至指定的子目錄*DirectoryPath*。|
|/t|指定移除的 8.3 檔案名稱應該在測試模式中執行。 執行所有作業，但無法實際移除 8.3 檔案名稱。 您可以使用 測試模式來探索哪些登錄機碼指向使用 8.3 檔案名稱的檔案。|
|/v|指定寫入記錄檔的所有資訊也會都顯示在命令列。|

## <a name="BKMK_remarks"></a>註解

-   永久移除 8.3 檔案名稱，並不會修改登錄機碼指向 8.3 檔案名稱可能會導致非預期的應用程式失敗，包括無法以解除安裝應用程式。 建議您先備份您的目錄或磁碟區在您嘗試將移除 8.3 檔案名稱。

## <a name="BKMK_examples"></a>範例
若要查詢的磁碟區 GUID，{928842df-5a01-11de-a85c-806e6f6e6963}，使用指定的停用 8.3 名稱行為中，輸入：

```
fsutil 8dot3name query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

您也可以使用查詢的 8.3 名稱行為**行為**子命令。

若要移除在 8.3 檔案名稱*D:\MyData*目錄和所有子目錄，同時將資訊寫入記錄檔指定為*mylogfile.log*，型別：

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fsutil 8dot3name](Fsutil-8dot3name.md)

[fsutil 行為](Fsutil-behavior.md)


