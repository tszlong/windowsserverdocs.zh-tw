---
title: diskpart 腳本和範例
description: 適用于 diskpart 腳本的參考文章，以及有關如何自動執行磁片相關工作的範例，例如建立磁片區或將磁片轉換成動態磁碟。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2715332a8a6adff29f31a4dbd7d3e7a27a4a6f2
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929325"
---
# <a name="diskpart-scripts-and-examples"></a>diskpart 腳本和範例

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用 `diskpart /s` 來執行腳本，以自動執行磁片相關工作，例如建立磁片區或將磁片轉換成動態磁碟。 如果您正使用自動安裝或 Sysprep 工具 (這不支援建立非開機磁碟區的磁碟區) 來部署 Windows，那麼以指令執行這些工作非常有用。

若要建立 diskpart 腳本，請建立一個文字檔，其中包含您想要執行的 Diskpart 命令，每行一個命令，而不是空白行。 您可以從開始一行， `rem` 讓這一行成為批註。 例如，以下腳本會抹除磁片，然後為 Windows 修復環境建立 300 MB 磁碟分割：

    ```
    select disk 0
    clean
    convert gpt
    create partition primary size=300
    format quick fs=ntfs label=Windows RE tools
    assign letter=T
    ```

## <a name="examples"></a>範例

- 若要執行 diskpart 腳本，請在命令提示字元中輸入下列命令，其中*scriptname*是包含腳本的文字檔名稱：

    ```
    diskpart /s scriptname.txt
    ```

- 若要將 diskpart 的腳本輸出重新導向至檔案，請輸入下列命令，其中*logfile*是 diskpart 寫入其輸出的文字檔名稱：

    ```
    diskpart /s scriptname.txt > logfile.txt
    ```

### <a name="remarks"></a>備註

- 在腳本中使用**diskpart**命令時，建議您將所有的 diskpart 作業一起完成，做為單一 diskpart 腳本的一部分。 您可以執行連續的 diskpart 腳本，但是在後續的腳本中再次執行**diskpart**命令之前，每個腳本必須至少要有15秒的時間，才能完全關閉先前的執行時間。 否則，後續的指令檔可能會失敗。 您可以在 `timeout /t 15` 批次檔中新增命令，連同您的 diskpart 腳本，以在連續的 diskpart 腳本之間新增暫停。

- 當 diskpart 啟動時，會在命令提示字元中顯示 diskpart 版本和電腦名稱稱。 根據預設，如果 diskpart 在嘗試執行腳本工作時遇到錯誤，則 diskpart 會停止處理腳本並顯示錯誤碼（除非您已指定**noerr**參數）。 不過，不論您是否使用了**noerr**參數，diskpart 一律會在遇到語法錯誤時傳回錯誤。 **Noerr**參數可讓您執行有用的工作，例如使用單一腳本刪除所有磁片上的所有分割區，而不論磁片總數為何。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [範例：使用 Windows PE 和 DiskPart 設定以 UEFI/GPT 為基礎的硬碟磁碟分割](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh825686(v=win.10))

- [範例：使用 Windows PE 和 DiskPart 設定以 BIOS/MBR 為基礎的硬碟磁碟分割](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh825677(v=win.10))

- [Windows PowerShell 中的儲存體 Cmdlet](https://docs.microsoft.com/powershell/module/storage/?view=win10-ps)
