---
title: Diskpart 腳本和範例
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c40ce79664795297af4369e35cbda7422617e6e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377848"
---
# <a name="diskpart-scripts-and-examples"></a>Diskpart 腳本和範例

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用 Diskpart `/s` 執行自動執行磁片\-相關工作的腳本，例如建立磁片區或將磁片轉換成動態磁碟。 如果您使用自動安裝或 Sysprep 工具（不支援建立開機磁碟區以外的磁片區）部署 Windows，則編寫這些工作的腳本會很有用。  
  
-   若要建立 Diskpart 腳本，請建立一個文字檔，其中包含您想要執行的 Diskpart 命令，每行一個命令，而不是空白行。 您可以從 `rem` 開始一行，讓這一行成為批註。  
  
    例如，這裡的腳本會抹除磁片，然後建立 Windows 修復環境的 300 MB 磁碟分割：  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label="Windows RE tools"  
    assign letter="T"  
    ```  
  
-   若要執行 DiskPart 腳本，請在命令提示字元中輸入下列命令，其中*scriptname*是包含腳本的文字檔名稱。  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   若要將 DiskPart 的腳本輸出重新導向至檔案，請輸入下列命令，其中*logfile*是 DiskPart 寫入其輸出的文字檔名稱。  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> 在腳本中使用**DiskPart**命令時，建議您將所有的 diskpart 作業一起完成，做為單一 DiskPart 腳本的一部分。 您可以執行連續的 DiskPart 腳本，但是在後續的腳本中再次執行**diskpart**命令之前，每個腳本必須至少要有15秒的時間，才能完全關閉先前的執行時間。 否則，後續的腳本可能會失敗。 您可以將 [`timeout /t 15`] 命令連同您的 DiskPart 腳本一起新增至批次檔，以在連續的 DiskPart 腳本之間新增暫停。  
  
當 DiskPart 啟動時，會在命令提示字元中顯示 DiskPart 版本和電腦名稱稱。 根據預設，如果 DiskPart 在嘗試執行腳本工作時遇到錯誤，則 DiskPart 會停止處理腳本，並顯示錯誤碼 \(除非您\)指定**noerr**參數。 不過，不論您是否使用了**noerr**參數，DiskPart 一律會在遇到語法錯誤時傳回錯誤。 **Noerr**參數可讓您執行有用的工作，例如使用單一腳本刪除所有磁片上的所有分割區，而不論磁片總數為何。  
  
## <a name="see-also"></a>請參閱  
[範例：使用 Windows PE 和 DiskPart 設定 UEFI\/gpt\-型硬碟磁碟分割](https://technet.microsoft.com/library/hh825686.aspx)  
[範例：使用 Windows PE 和 DiskPart 設定 BIOS\/MBR\-型硬碟磁碟分割](https://technet.microsoft.com/library/hh825677.aspx)  
[Windows PowerShell 中的儲存體 Cmdlet](https://technet.microsoft.com/library/hh848705.aspx)  
  

