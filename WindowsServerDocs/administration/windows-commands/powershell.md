---
title: PowerShell
description: 了解如何從命令提示字元中開啟 PowerShell 主控台。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c8070268fdf58fbbb71c159a7360b488222ef740
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852179"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell 是以工作為基礎的命令列殼層和指令碼語言，專為系統管理。 Windows PowerShell 是以 .NET Framework 為基礎所建置，可協助 IT 專業人員與進階使用者控制和自動化管理 Windows 作業系統與在 Windows 上執行的應用程式。

**PowerShell.exe**命令列工具的命令提示字元視窗中啟動 Windows PowerShell 工作階段。 當您使用**PowerShell.exe**，您可以使用其選擇性參數來自訂工作階段。 例如，您可以開始使用特定的執行原則] 或 [排除的 Windows PowerShell 設定檔的其中一個的工作階段。 否則，工作階段等同於在 Windows PowerShell 主控台中啟動任何工作階段。

## <a name="using-powershellexe"></a>使用 PowerShell.exe

您可以使用**PowerShell.exe**命令列工具來啟動 Windows PowerShell 工作階段在命令提示字元 視窗中。

- 若要啟動 Windows PowerShell 工作階段在命令提示字元 視窗中，輸入`PowerShell`。 A **PS**前置詞會加入至命令提示字元，以指出您是在 Windows PowerShell 工作階段。
- 若要啟動工作階段與特定的執行原則，使用**ExecutionPolicy**參數。  
    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```  
- 若要啟動 Windows PowerShell 工作階段，而不需要您的 Windows PowerShell 設定檔，請使用**NoProfile**參數。  
    ```
    PowerShell.exe -NoProfile
    ```  
- 若要啟動工作階段，使用**ExecutionPolicy**參數。  
    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```  
- 若要查看 PowerShell.exe 說明檔，請使用下列命令格式。  
    ```
    PowerShell.exe -help, -?, /?
    ```  
- 若要結束 Windows PowerShell 工作階段在命令提示字元 視窗中，輸入`exit`。 會傳回一般的命令提示字元。

如需完整的清單**PowerShell.exe**命令列參數，請參閱[about_PowerShell.Exe](https://go.microsoft.com/fwlink/?LinkID=113439)。

## <a name="other-ways-to-start-windows-powershell"></a>若要啟動 Windows PowerShell 的其他方式

若要啟動 Windows PowerShell 的其他方式的相關資訊，請參閱[啟動 Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259)。

## <a name="remarks"></a>備註

在 Windows Server 作業系統的 Server Core 安裝選項上，執行 Windows PowerShell。 不過，功能需要圖形化使用者介面，例如[Windows PowerShell 整合式指令碼環境 (ISE)](https://technet.microsoft.com/library/hh849182)，而[Out-gridview](https://go.microsoft.com/fwlink/?LinkID=113364)和[Show-command](https://go.microsoft.com/fwlink/?LinkID=217448)cmdlet，無法執行 Server Core 安裝上。

## <a name="additional-references"></a>其他參考資料

[about_PowerShell.Exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[使用 Windows 撰寫指令碼PowerShell](https://technet.microsoft.com/scriptcenter/dd742419)另請參閱