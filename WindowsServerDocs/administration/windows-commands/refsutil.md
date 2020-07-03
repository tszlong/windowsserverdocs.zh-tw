---
title: ReFSUtil
description: '[ReFSUtil] 工具的參考文章，會嘗試診斷嚴重損毀的 ReFS 磁片區、識別剩餘的檔案，並將這些檔案複製到另一個磁片區。'
author: laknight5
ms.author: laknight
ms.date: 6/29/2020
ms.prod: windows-server
ms.technology: storage-file-systems
ms.topic: article
ms.openlocfilehash: c84aaed5b34c535221247dcdd6ab0a5462fd4aad
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931107"
---
# <a name="refsutil"></a>ReFSUtil

>適用於：Windows Server 2019、Windows 10

ReFSUtil 是 Windows 和 Windows Server 中包含的工具，會嘗試診斷嚴重損毀的 ReFS 磁片區、識別剩餘的檔案，並將這些檔案複製到另一個磁片區。 這是在 Windows 10 的資料夾中， `%SystemRoot%\Windows\System32` 或是在 Windows Server 的 `%SystemRoot%\\System32` 資料夾中。

ReFS 搶救是 ReFSUtil 的主要功能，適用于從磁片管理中顯示為原始的磁片區復原資料。 ReFS 搶救有兩個階段：掃描階段和複製階段。 在 [自動] 模式中，[掃描] 階段和 [複製階段] 將會循序執行。 在手動模式中，每個階段都可以單獨執行。 進度和記錄會儲存在工作目錄中，以允許個別執行階段，以及要暫停和繼續掃描階段。 您不應該使用 ReFSutil 工具，除非該磁片區是 RAW。 如果是唯讀，則資料仍然可以存取。

## <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<source volume>` | 指定要處理的 ReFS 磁片區。 磁碟機號必須格式化為 "L："，或者您必須提供磁片區掛接點的路徑。 |
| `<working directory>` | 指定要儲存暫存資訊和記錄檔的位置。 它不得**位於**上 `<source volume>` 。 |
| `<target directory>` | 指定要將已識別檔案複製到其中的位置。 它不得**位於**上 `<source volume>` 。 |
| \-m | 復原所有可能的檔案，包括刪除的檔案。<p>**警告：** 這個參數不僅會使進程執行較長的時間，也可能導致非預期的結果。 |
| \-& | 指定使用詳細資訊模式。 |
| \-x | 必要時，強制先卸載磁片區。 磁片區的所有開啟的控制碼都將失效。 例如 `refsutil salvage -QA R: N:\\WORKING N:\\DATA -x`。 |

## <a name="usage-and-available-options"></a>使用方式和可用的選項

### <a name="quick-automatic-mode-command-line-usage"></a>快速自動模式命令列使用方式

執行快速掃描階段，然後進行複製階段。 這種模式的執行速度會更快，因為它假設磁片區的某些重要結構並未損毀，因此不需要掃描整個磁片區來找出它們。 這也會減少過時檔案/目錄/磁片區的復原。

```
refsutil salvage -QA <source volume> <working directory> <target directory> <options>
```

### <a name="full-automatic-mode-command-line-usage"></a>完整自動模式命令列使用方式

執行完整的掃描階段，後面接著複製階段。 此模式可能需要很長的時間，因為它會掃描整個磁片區中是否有任何可復原的檔案/目錄/磁片區。

```
refsutil salvage -FA <source volume> <working directory> <target directory> <options>
```

### <a name="diagnose-phase-command-line-usage-manual-mode"></a>診斷階段命令列使用方式（手動模式）

首先，請嘗試判斷 `<source volume>` 是否為 ReFS 磁片區，並判斷磁片區是否為可裝載。 如果磁片區無法裝載，則會提供原因。 這是獨立的階段。

```
refsutil salvage -D <source volume> <working directory> <options>
```

### <a name="quick-scan-phase-command-line-usage"></a>快速掃描階段命令列使用方式

針對任何可復原的檔案執行快速掃描 `<source volume>` 。 這種模式的執行速度會更快，因為它假設磁片區的某些重要結構並未損毀，因此不需要掃描整個磁片區來找出它們。 這也會減少過時檔案/目錄/磁片區的復原。 探索到的檔案會記錄到檔案 `foundfiles.<volume signature>.txt` 中，位於您的中 `<working directory>` 。 如果掃描階段先前已停止，使用 **-QS**旗標執行時，會再次從中斷處繼續掃描。

```
refsutil salvage -QS <source volume> <working directory> <options>
```

### <a name="full-scan-phase-command-line-usage"></a>完整掃描階段命令列使用方式

掃描整個 `<source volume>` 是否有可復原的檔案。 此模式可能需要很長的時間，因為它會掃描整個磁片區中是否有任何可復原的檔案。 探索到的檔案將會記錄到檔案 `foundfiles.<volume signature>.txt` 中，位於您的中 `<working directory>` 。 如果掃描階段先前已停止，使用 **-FS**旗標執行時，會從中斷處繼續掃描。

```
refsutil salvage -FS <source volume> <working directory> <options>
```

### <a name="copy-phase-command-line-usage"></a>複製階段命令列使用方式

將檔案中描述的所有檔案複製 `foundfiles.<volume signature>.txt` 到您的 `<target directory>` 。 如果您過早停止掃描階段，可能是檔案尚未 `foundfiles.<volume signature>.txt` 存在，因此不會將任何檔案複製到 `<target directory>` 。

```
refsutil salvage -C <source volume> <working directory> <target directory> <options>
```

### <a name="copy-phase-with-list-command-line-usage"></a>具有清單命令列使用方式的複製階段

將中的所有檔案複製 `<file list>` `<source volume>` 到您的 `<target directory>` 。 中的檔案 `<file list>` 必須先由掃描階段識別，但掃描不需要執行到完成。 `<file list>`可以藉由複製 `foundfiles.<volume signature>.txt` 到新的檔案來產生，移除參考不應還原之檔案的行，並保留應該還原的檔案。 PowerShell Cmdlet **Select 字串**可能有助於篩選， `foundfiles.<volume signature>.txt` 只包含所需的路徑、副檔名或檔案名。

```
refsutil salvage -SL <source volume> <working directory> <target directory> <file list> <options>
```

### <a name="copy-phase-with-interactive-console"></a>使用互動式主控台複製階段

Advanced users 可以使用互動式主控台搶救檔案。 此模式也需要從任一掃描階段產生的檔案。

```
refsutil salvage -IC <source volume> <working directory> <options>
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
