---
title: Diskshadow
description: Diskshadow 命令的參考主題，這是一種可公開磁片區陰影複製服務（VSS）所提供之功能的工具。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae3a4ba57d9c29375c560c300a4e4ead807184fc
ms.sourcegitcommit: aed942d11f1a361fc1d17553a4cf190a864d1268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83235194"
---
# <a name="diskshadow"></a>Diskshadow

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Diskshadow 是一種工具，可公開磁片區陰影複製服務（VSS）所提供的功能。 根據預設，Diskshadow 會使用類似于 Diskraid 或 Diskpart 的互動式命令直譯器。 Diskshadow 也包含可編寫腳本的模式。

> [!NOTE]
> 若要執行 Diskshadow，至少需要本機 Administrators 群組的成員資格或同等許可權。

## <a name="syntax"></a>語法

針對互動模式，請在命令提示字元中輸入下列命令，以啟動 Diskshadow 命令直譯器：

```
diskshadow
```

針對 [腳本模式]，輸入下列程式碼，其中*script .txt*是包含 Diskshadow 命令的腳本檔案：

```
diskshadow -s script.txt
```

### <a name="parameters"></a>參數

您可以在 Diskshadow 命令直譯器中或透過腳本檔案執行下列命令。 至少要有**add**和**create** ，才可建立陰影複製。 不過，這會 forfeits 內容和選項設定、將會是複本備份，並建立不含備份執行腳本的陰影複製。

| 命令 | 說明 |
| --------- | ----------- |
| [set 命令](set_2.md) | 設定用來建立陰影複製的內容、選項、詳細資訊模式和中繼資料檔案。 |
| [載入中繼資料命令](load-metadata.md) | 在匯入可轉移的陰影複製之前載入中繼資料 .cab 檔案，或在還原時載入寫入器中繼資料。 |
| [寫入器命令](writer.md) | 確認已包含寫入器或元件，或從備份或還原程式中排除寫入器或元件。 |
| [新增命令](add.md) | 將磁片區新增至要陰影複製的磁片區集合，或將別名新增至別名環境。 |
| [建立命令](create.md) | 使用目前的內容和選項設定，啟動陰影複製建立進程。 |
| [exec 命令](exec.md) | 在本機電腦上執行檔案。 |
| [開始備份命令](begin-backup.md) | 啟動完整備份會話。 |
| [結束備份命令](end-backup.md) | 結束完整的備份會話，並視需要發出具有適當寫入器狀態的**backupcomplete**事件。 |
| [開始還原命令](begin-restore.md) | 啟動還原會話，並將**prerestore**事件發出至相關的寫入器。 |
| [結束還原命令](end-restore.md) | 結束還原會話，並向相關的寫入器發出**postrestore**事件。 |
| [重設命令](reset.md) | 將 Diskshadow 重設為預設狀態。 |
| [list 命令](list.md) | 列出系統上的寫入器、陰影複製或目前已註冊的陰影複製提供者。 |
| [刪除陰影命令](delete-shadows.md) | 刪除陰影複製。 |
| [匯入命令](import.md) | 從載入的中繼資料檔案，將可轉移的陰影複製匯入到系統中。 |
| [mask 命令](mask.md) | 移除使用匯**入**命令匯入的硬體陰影複製。 |
| [公開命令](expose.md) | 將持續性陰影複製公開為磁碟機號、共用或掛接點。 |
| [diskshadow.exe 取消公開命令](unexpose.md) | Unexposes 使用**公開**命令公開的陰影複製。 |
| [break 命令](break_2.md) | 將陰影複製磁片區與 VSS 解除。 |
| [還原命令](revert.md) | 將磁片區還原回指定的陰影複製。 |
| [exit 命令](exit.md) | 結束命令直譯器或腳本。 |

## <a name="examples"></a>範例

這是命令的範例順序，會建立備份的陰影複製。 它可以儲存為 dsh 檔案，並使用執行 `diskshadow /s script.dsh` 。

假設下列各項：

- 您有一個名為 c： diskshadowdata 的現有目錄 \\ 。

- 您的系統磁碟區是 C：，而您的資料磁片區是 d：

- 您在 c： diskshadowdata 中有 backupscript .cmd 檔案 \\ 。

- 您的 backupscript .cmd 檔案會將陰影資料 p：和 q：的複本執行到您的備份磁片磁碟機。

您可以手動輸入這些命令或編寫它們的腳本：

```
#Diskshadow script file
set context persistent nowriters
set metadata c:\diskshadowdata\example.cab
set verbose on
begin backup
add volume c: alias systemvolumeshadow
add volume d: alias datavolumeshadow

create

expose %systemvolumeshadow% p:
expose %datavolumeshadow% q:
exec c:\diskshadowdata\backupscript.cmd
end backup
#End of script
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
