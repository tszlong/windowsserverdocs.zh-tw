---
title: ReFSUtil
description: ReFSUtil 是 Windows 和 Windows Server 中包含的工具，會嘗試診斷嚴重損毀的 ReFS 磁片區、識別剩餘的檔案，並將這些檔案複製到另一個磁片區。
author: laknight5
ms.author: laknight
ms.date: 6/29/2020
ms.prod: windows-server
ms.technology: storage-file-systems
ms.topic: article
ms.openlocfilehash: 0d7c1bf79695c2ddd03943744ebf1df4827d365c
ms.sourcegitcommit: 457e88e5aa6be13a2bffdb8e434a8efc3698678f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/30/2020
ms.locfileid: "85549143"
---
# <a name="refsutil"></a>ReFSUtil

>適用於：Windows Server 2019、Windows 10

ReFSUtil 是 Windows 和 Windows Server 中包含的工具，會嘗試診斷嚴重損毀的 ReFS 磁片區、識別剩餘的檔案，並將這些檔案複製到另一個磁片區。 這是在 Windows 10 的%SystemRoot%\Windows\System32 資料夾中，或是在 Windows Server 的% SystemRoot% \\ System32 資料夾中。

ReFS 搶救是目前的 [ReFSUtil] 主要功能，適用于從磁片管理中顯示為 [原始] 的磁片區復原資料。 ReFS 搶救有兩個階段：掃描階段和複製階段。 在 [自動] 模式中，[掃描] 階段和 [複製階段] 將會循序執行。 在手動模式中，每個階段都可以單獨執行。 進度和記錄會儲存在工作目錄中，以允許個別執行階段，以及要暫停和繼續掃描階段。 除非磁片區是 RAW，否則不需要使用 ReFSutil。 如果是唯讀，則資料仍然可以存取。

## <a name="automatic-mode-command-line-usage"></a>自動模式命令列使用方式

### <a name="quick-automatic-mode-command-line-usage"></a>快速自動模式命令列使用方式

```dos
refsutil salvage -QA <source volume> <working directory> <target directory> <options>
```
它會執行快速掃描階段，後面接著複製階段。 這種模式的執行速度會更快，因為它假設磁片區的某些重要結構並未損毀，因此不需要掃描整個磁片區來找出它們。 這也會減少過時檔案/目錄/磁片區的復原。

### <a name="full-automatic-mode-command-line-usage"></a>完整自動模式命令列使用方式

```dos
refsutil salvage -FA <source volume> <working directory> <target directory> <options>
```

它將會執行完整的掃描階段，然後進行複製階段。 此模式可能需要很長的時間，因為它會掃描整個磁片區中是否有任何可復原的檔案/目錄/磁片區。

## <a name="manual-mode-command-line-usage"></a>手動模式命令列使用方式

### <a name="diagnose-phase-command-line-usage"></a>診斷階段命令列使用方式

```dos
refsutil salvage -D <source volume> <working directory> <options>
```

首先，請嘗試判斷 \<source volume\> 是否為 ReFS 磁片區，並判斷磁片區是否為可裝載。 當磁片區無法裝載時，將會決定原因。 這是獨立的階段。

### <a name="quick-scan-phase-command-line-usage"></a>快速掃描階段命令列使用方式

```dos
refsutil salvage -QS <source volume> <working directory> <options>
```

快速掃描 \<source volume\> 任何可復原的檔案。 這種模式的執行速度會更快，因為它假設磁片區的某些重要結構並未損毀，因此不需要掃描整個磁片區來找出它們。 這也會減少過時檔案/目錄/磁片區的復原。 探索到的檔案將會記錄到 "foundfiles. \<volume signature\> 。txt " \<working directory\> 。 如果掃描階段先前已停止，則再次使用-QS 旗標執行，將會從中斷處繼續掃描。

### <a name="full-scan-phase-command-line-usage"></a>完整掃描階段命令列使用方式

```dos
refsutil salvage -FS <source volume> <working directory> <options>
```

\<source volume\>為所有可復原的檔案掃描整個。 此模式可能需要很長的時間，因為它會掃描整個磁片區中是否有任何可復原的檔案。
探索到的檔案將會記錄到 "foundfiles. \<volume signature\> 。txt " \<working directory\> 。 如果掃描階段先前已停止，則使用-FS 旗標再次執行，將會從中斷處繼續掃描。

### <a name="copy-phase-command-line-usage"></a>複製階段命令列使用方式

```dos
refsutil salvage -C <source volume> <working directory> <target directory>
<options>
```

複製 "foundfiles" 中所述的所有 \<volume signature\> 檔案。txt "至 \<target
directory\> 。 如果掃描階段太早停止，請 \<volume
signature\> foundfiles。txt "可能尚未寫入，因此不會將任何檔案複製到其中 \<target directory\> 。

### <a name="copy-phase-with-list-command-line-usage"></a>具有清單命令列使用方式的複製階段

```dos
refsutil salvage -SL <source volume> <working directory> <target
directory> <file list> <options>
```

將中的所有檔案 \<file list\> 複製 \<source volume\> 到 \<target
directory\> 。 中的檔案 \<file list\> 必須先由掃描階段識別，但掃描不需要執行到完成。 \<file list\>可以藉由複製 "foundfiles" 來產生 \<volume signature\> 。txt 「到新檔案，移除參考不應還原之檔案的行，並保留應該還原的檔案。 PowerShell Cmdlet Select 字串可能有助於篩選 "foundfiles \<volume signature\> "。txt "，只包含所需的路徑、副檔名或檔案名。

### <a name="copy-phase-with-interactive-console"></a>使用互動式主控台複製階段

```dos
refsutil salvage -IC <source volume> <working directory> <options>
```

在互動式主控台中為 advanced users 回收檔案。 此模式也需要從任一掃描階段產生的檔案。

## <a name="parameters"></a>參數

| 參數             | 描述                                                                     |
| --------------------- | --------------------------------------------------------------------------------------- |
| \<source volume\>     | 要處理的 ReFS 磁片區。 格式為 "L：" 的磁碟機號，或磁片區掛接點的路徑。           |
| \<working directory\> | 儲存臨時資訊和記錄檔的位置。 **不得位於** \<source volume\> 。  |
| \<target directory\>  | 已識別的檔案將會複製到其中的位置。 **不得位於** \<source volume\> 。 |
| \-m         | 復原所有可能的檔案，包括已刪除的檔案。 在 refsutil 搶救 v1 上會忽略此選項。 警告：這種情況不只需要較長的時間執行，可能會導致非預期的結果。 |
| \-&         | 詳細資訊模式                                                                                           |
| \-x         | 必要時，強制先卸載磁片區。 磁片區的所有開啟的控制碼都將會無效。 例如： refsutil 搶救-QA R： N： \\ 工作 N： \\ 資料-x                                  |
