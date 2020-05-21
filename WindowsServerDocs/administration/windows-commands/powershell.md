---
title: PowerShell
description: 瞭解如何從命令提示字元開啟 PowerShell 主控台。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: b733a187017293e8a33ff307b485380ef8f9b472
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436563"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell 是以工作為基礎的命令列 shell 與指令碼語言，專為系統管理所設計。 Windows PowerShell 是以 .NET Framework 為基礎所建置，可協助 IT 專業人員與進階使用者控制和自動化管理 Windows 作業系統與在 Windows 上執行的應用程式。

**PowerShell .exe**命令列工具會在 [命令提示字元] 視窗中啟動 Windows PowerShell 會話。 當您使用**PowerShell**時，您可以使用其選擇性參數來自訂會話。 例如，您可以啟動使用特定執行原則或排除 Windows PowerShell 設定檔的會話。 否則，會話會與 Windows PowerShell 主控台中啟動的任何會話相同。

## <a name="using-powershellexe"></a>使用 PowerShell .exe

您可以使用**PowerShell .exe**命令列工具，在 [命令提示字元] 視窗中啟動 Windows PowerShell 會話。

- 若要在 [命令提示字元] 視窗中啟動 Windows PowerShell 會話，請輸入 `PowerShell` 。 會在命令提示字元中新增**PS**首碼，以指出您是在 Windows PowerShell 會話中。

- 若要以特定執行原則啟動會話，請使用**ExecutionPolicy**參數。

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- 若要在沒有 Windows PowerShell 設定檔的情況下啟動 Windows PowerShell 會話，請使用**NoProfile**參數。

    ```
    PowerShell.exe -NoProfile
    ```

- 若要啟動會話，請使用**ExecutionPolicy**參數。

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- 若要查看 PowerShell .exe 說明檔，請使用下列命令格式。

    ```
    PowerShell.exe -help, -?, /?
    ```

- 若要在 [命令提示字元] 視窗中結束 Windows PowerShell 會話，請輸入 `exit` 。 一般的命令提示字元會傳回。

如需完整的**PowerShell .exe**命令列參數清單，請參閱[about_PowerShell .exe](https://go.microsoft.com/fwlink/?LinkID=113439)。

## <a name="other-ways-to-start-windows-powershell"></a>啟動 Windows PowerShell 的其他方式

如需啟動 Windows PowerShell 的其他方式的詳細資訊，請參閱[啟動 Windows powershell](https://go.microsoft.com/fwlink/?LinkID=135259)。

## <a name="remarks"></a>備註

Windows PowerShell 會在 Windows Server 作業系統的 Server Core 安裝選項上執行。 不過，需要圖形使用者介面的功能（例如[Windows PowerShell 整合式腳本環境（ISE）](https://technet.microsoft.com/library/hh849182)，以及[Out-GridView](https://go.microsoft.com/fwlink/?LinkID=113364)和[Show 命令](https://go.microsoft.com/fwlink/?LinkID=217448)Cmdlet）不會在 Server Core 安裝上執行。

## <a name="additional-references"></a>其他參考

[About_PowerShell .exe](https://go.microsoft.com/fwlink/?LinkID=113439) 
[about_PowerShell_Ise .exe](https://go.microsoft.com/fwlink/?LinkId=256512) 
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116) 
[使用 Windows PowerShell 撰寫腳本](https://technet.microsoft.com/scriptcenter/dd742419)另請參閱