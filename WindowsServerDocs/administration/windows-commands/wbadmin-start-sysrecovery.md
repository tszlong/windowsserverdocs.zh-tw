---
title: wbadmin start sysrecovery
description: Wbadmin start sysrecovery 的參考文章，它會使用您指定的參數執行系統復原 (裸機復原) 。
ms.topic: reference
ms.assetid: 95b8232f-7c42-452b-838e-15b0cf6faebe
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e9cac744465d2744217ec1da594f41c27ffe317d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621501"
---
# <a name="wbadmin-start-sysrecovery"></a>wbadmin start sysrecovery



使用您指定的參數，執行系統復原 (裸機復原) 。

> [!NOTE]
> 這個子命令只能從 Windows 修復環境執行，而且預設不會列在 **Wbadmin**的使用方式文字中。 如需詳細資訊，請參閱 [Windows 修復環境 (Windows RE) 總覽](/previous-versions/windows/it-pro/windows-8.1-and-8/hh825173(v=win.10))。

若要使用這個子命令執行系統復原，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。

## <a name="syntax"></a>語法

```
wbadmin start sysrecovery
-version:<VersionIdentifier>
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-restoreAllVolumes]
[-recreateDisks]
[-excludeDisks]
[-skipBadClusterCheck]
[-quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-version|以 MM/DD/YYYY-HH： MM 格式指定要復原的備份版本識別碼。 如果您不知道版本識別碼，請輸入 **wbadmin get**version。|
|-backupTarget|指定含有您要復原的備份的存放位置。 當儲存位置與此電腦的備份通常儲存位置不同時，此參數很有用。|
|-電腦|指定您想要復原的電腦名稱稱。 當多部電腦備份至相同的位置時，此參數很有用。 當指定 **-backupTarget** 參數時，應該使用。|
|-restoreAllVolumes|復原所選備份中的所有磁片區。 如果未指定此參數，則只會復原包含系統狀態和作業系統元件) 的重要磁片區 (磁片區。 當您需要在系統復原期間復原非重要磁片區時，此參數很有用。|
|-recreateDisks|將磁片設定復原到建立備份時存在的狀態。</br>警告：此參數會刪除裝載作業系統元件之磁片區上的所有資料。 它也可能會刪除資料磁片區中的資料。|
|-excludeDisks|只有在使用 **-recreateDisks** 參數指定時才有效，而且必須以逗號分隔的磁片識別碼清單來輸入， (如 **wbadmin get** disk) 的輸出中所列。 排除的磁片未分割或格式化。 此參數有助於保留您在復原作業期間不想修改之磁片上的資料。|
|-skipBadClusterCheck|略過檢查修復磁片是否有錯誤的叢集資訊。 如果您要還原至替代伺服器或硬體，建議您不要使用此參數。 您可以隨時在復原磁片上手動執行 **chkdsk/b** 來檢查是否有錯誤的叢集，然後據以更新檔案系統資訊。</br>警告：在您執行 **Chkdsk** 之前（如所述），復原系統上回報的錯誤叢集可能不正確。|
|-quiet|以不提示使用者的方式執行命令。|

## <a name="examples"></a>範例

若要在2013年3月31日上午9:00 開始復原執行的備份資訊，請在磁片磁碟機 d：上輸入：
```
wbadmin start sysrecovery -version:03/31/2013-09:00 -backupTarget:d:
```
若要在2013年4月30日上午9:00 開始復原執行的備份資訊，請在共用資料夾 \\ \\ servername\shared：針對 server01 輸入：
```
wbadmin start sysrecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Backup](wbadmin.md)
-   [WBBareMetalRecovery](/previous-versions/windows/it-pro/windows-8.1-and-8/hh825173(v=win.10)) 指令 Cmdlet
