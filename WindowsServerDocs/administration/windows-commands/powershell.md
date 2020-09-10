---
title: PowerShell
description: PowerShell 命令的參考文章，此命令會從命令提示字元開啟 PowerShell 主控台。
ms.topic: reference
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 3cfe7a9bada34a2678f2c63e70f698b018478770
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627229"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell 是以工作為基礎的命令列 shell 和指令碼語言，專為系統管理所設計。 Windows PowerShell 是以 .NET Framework 為基礎所建置，可協助 IT 專業人員與進階使用者控制和自動化管理 Windows 作業系統與在 Windows 上執行的應用程式。

## <a name="using-powershellexe"></a>使用 PowerShell.exe

**PowerShell.exe**命令列工具會在 [命令提示字元] 視窗中啟動 Windows PowerShell 會話。 當您使用 **PowerShell.exe**時，您可以使用其選擇性參數來自訂會話。 例如，您可以啟動使用特定執行原則的會話，或啟動一個排除 Windows PowerShell 設定檔的會話。 否則，會話與 Windows PowerShell 主控台中啟動的任何會話相同。

- 若要在 [命令提示字元] 視窗中啟動 Windows PowerShell 會話，請輸入 `PowerShell` 。 系統會將 **PS** 前置詞新增至命令提示字元，以指出您是在 Windows PowerShell 會話中。

- 若要啟動具有特定執行原則的會話，請使用 **ExecutionPolicy** 參數，然後輸入：

    ```powershell
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- 若要在不使用 Windows PowerShell 設定檔的情況下啟動 Windows PowerShell 會話，請使用 **NoProfile** 參數，然後輸入：

    ```powershell
    PowerShell.exe -NoProfile
    ```

- 若要啟動會話，請使用 **ExecutionPolicy** 參數，然後輸入：

    ```powershell
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- 若要查看 PowerShell.exe 說明檔，請輸入：

    ```powershell
    PowerShell.exe -help
    PowerShell.exe -?
    PowerShell.exe /?
    ```

- 若要在命令提示字元視窗中結束 Windows PowerShell 的會話，請輸入 `exit` 。 一般的命令提示字元會傳回。

### <a name="remarks"></a>備註

- 如需 **PowerShell.exe** 命令列參數的完整清單，請參閱 [about_PowerShell.Exe](/powershell/module/microsoft.powershell.core/about/about_powershell_exe)。

- 如需其他開始 Windows PowerShell 方式的詳細資訊，請參閱 [開始 Windows PowerShell](/powershell/scripting/windows-powershell/starting-windows-powershell)。

- Windows PowerShell 在 Windows Server 作業系統的 Server Core 安裝選項上執行。 不過，需要圖形使用者介面的功能（例如 [Windows PowerShell 整合式腳本環境 (ISE) ](/previous-versions/hh849182(v=technet.10))）和 [非 GridView](/powershell/module/microsoft.powershell.utility/out-gridview) 和 [Show 命令](/powershell/module/microsoft.powershell.utility/show-command) Cmdlet 都不會在 Server Core 安裝上執行。

## <a name="additional-references"></a>其他參考資料

- [about_PowerShell.Exe](/powershell/module/microsoft.powershell.core/about/about_powershell_exe)

- [about_PowerShell_Ise.exe](/powershell/module/microsoft.powershell.core/about/about_powershell_ise_exe)

- [Windows PowerShell](/powershell/)
