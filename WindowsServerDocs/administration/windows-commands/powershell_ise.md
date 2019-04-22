---
title: PowerShell_ise
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03c765b276a2e61247661e132dd49434b444530c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817279"
---
# <a name="powershellise"></a>PowerShell_ise



Windows PowerShell 整合式指令碼環境 (ISE) 是可讓您讀取、 寫入、 執行、 偵錯和測試指令碼和模組圖形輔助的環境中的圖形化的主機應用程式。 主要功能，例如 IntelliSense、 顯示命令、 程式碼片段、 tab 鍵自動完成、 語法著色、 視覺化偵錯，和提供豐富的指令碼撰寫體驗的即時線上說明。

**PowerShell_ISE.exe**工具啟動 Windows PowerShell ISE 工作階段。 當您使用**PowerShell_ISE.exe**，在 Windows PowerShell ISE 中開啟檔案，或不含設定檔，或使用多執行緒 apartment 啟動 Windows PowerShell ISE 工作階段，您可以使用其選擇性參數。

**PowerShell_ISE.exe**已在 Windows PowerShell 2.0 引進，並在 Windows PowerShell 3.0 大幅擴充。

## <a name="using-powershelliseexe"></a>使用 PowerShell_ISE.exe

您可以使用**PowerShell_ISE.exe**開始和結束 Windows PowerShell 工作階段，如下所示：
-   若要啟動 Windows PowerShell ISE 工作階段，在命令提示字元視窗中，在 Windows PowerShell 中，或在 [開始] 功能表中，輸入：  
    ```
    PowerShell_Ise
    ```  
-   若要在 Windows PowerShell ISE 中開啟指令碼 (.ps1)、 指令碼模組 (.psm1)、 模組資訊清單 (.psd1)、 XML 檔案或任何其他支援的檔案，請使用下列命令格式：  
    ```
    PowerShell_Ise <FilePath>
    ```  
    在 Windows PowerShell 3.0 中，您可以使用選擇性**檔案**參數，如下所示：  
    ```
    PowerShell_Ise -File <FilePath>
    ```  
-   若要啟動的 Windows PowerShell ISE 工作階段，而不需要您的 Windows PowerShell 設定檔，請使用**NoProfile**參數。 ( **NoProfile**參數在 Windows PowerShell 3.0 引進。)  
    ```
    PowerShell_Ise -NoProfile
    ```  
-   若要查看**PowerShell_ISE.exe**協助檔案中的命令提示字元視窗，請使用下列命令格式：  
    ```
    PowerShell_Ise -help, -?, /?
    ```  
如需完整的清單**PowerShell_ISE.exe**命令列參數，請參閱[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)。

## <a name="start-windows-powershell-ise-in-other-ways"></a>其他方法啟動 Windows PowerShell ISE

若要啟動 Windows PowerShell ISE 的其他方式的相關資訊，請參閱[啟動 Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259)。

## <a name="remarks"></a>備註

在 Windows Server 作業系統的 Server Core 安裝選項上，執行 Windows PowerShell。 不過，Windows PowerShell ISE 需要圖形化使用者介面，因為它無法執行 Server Core 安裝上。

## <a name="additional-references"></a>其他參考資料

[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[about_PowerShell.exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[使用 Windows 撰寫指令碼PowerShell](https://technet.microsoft.com/scriptcenter/dd742419)另請參閱