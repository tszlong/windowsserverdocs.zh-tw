---
title: Diskpart 指令碼和範例
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a8bd0dfab98ca705cf64766d66285c69bd3d3129
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862789"
---
# <a name="diskpart-scripts-and-examples"></a>Diskpart 指令碼和範例

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

使用 Diskpart`/s`執行指令碼自動執行磁碟\-相關工作，例如建立磁碟區，或將磁碟轉換為動態磁碟。 指令執行這些工作是很有用，如果您使用來部署 Windows 自動的安裝或 Sysprep 工具，不支援建立非開機磁碟區的磁碟區。  
  
-   若要建立的 Diskpart 指令碼，建立文字檔，其中包含您想要執行，每行一個命令與沒有空白行 Diskpart 命令。 您可以啟動某一行的`rem`讓線條註解。  
  
    比方說，這裡 s 清除磁碟，然後建立 300 MB 的磁碟分割的 Windows 修復環境的指令碼：  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label="Windows RE tools"  
    assign letter="T"  
    ```  
  
-   若要執行的 DiskPart 指令碼，在命令提示字元中，輸入下列命令，其中*scriptname*是文字檔，其中包含您的指令碼的名稱。  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   若要將 DiskPart 的指令碼輸出重新導向至檔案，輸入下列命令，其中*logfile*是 DiskPart 寫入其輸出的位置的文字檔案的名稱。  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> 使用時**DiskPart**命令的一部分指令碼中，我們建議您先完成所有的 DiskPart 作業一起做為單一的 DiskPart 指令碼的一部分。 您可以執行連續的 DiskPart 指令檔，但您必須允許至少 15 秒之間以完整關機，再執行上次執行的每個指令碼**DiskPart**命令一次在後續的指令碼中。 否則，後續的指令碼可能會失敗。 您可以將連續的 DiskPart 指令碼，藉由新增之間暫停`timeout /t 15`命令以將批次檔，以及您的 DiskPart 指令碼。  
  
啟動 DiskPart 時，DiskPart 版本及電腦名稱會顯示在命令提示字元。 根據預設，如果 DiskPart 而發生錯誤時嘗試執行指令碼的工作中，DiskPart 會停止處理指令碼，並顯示錯誤碼\(除非您指定了**noerr**參數\)。 不過，DiskPart 一律會傳回錯誤發生語法錯誤，不論您是否使用時**noerr**參數。 **Noerr**參數可讓您執行有用的工作，例如使用單一指令碼，若要刪除的磁碟總數無論的所有磁碟上的所有資料分割。  
  
## <a name="see-also"></a>另請參閱  
[範例：設定 UEFI\/gpt\-型硬碟磁碟機資料分割所使用的 Windows PE 和 DiskPart](https://technet.microsoft.com/library/hh825686.aspx)  
[範例：設定 BIOS\/MBR\-使用 Windows PE 和 DiskPart 型硬碟磁碟分割](https://technet.microsoft.com/library/hh825677.aspx)  
[Windows PowerShell 中的儲存體 Cmdlet](https://technet.microsoft.com/library/hh848705.aspx)  
  

