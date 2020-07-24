---
title: PowerShell_ise
description: PowerShell_ise 命令的參考文章，它會啟動 Windows PowerShell 整合式腳本環境（ISE）會話。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 24fc3c6dca5ba3fea872f625b2ef81f1c78f59fb
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956570"
---
# <a name="powershell_ise"></a>PowerShell_ise

Windows PowerShell 整合式腳本環境（ISE）是一個圖形化主機應用程式，可讓您在圖形輔助環境中讀取、寫入、執行、偵測及測試腳本和模組。 IntelliSense、顯示命令、程式碼片段、tab 鍵自動完成、語法著色、視覺化調試和即時線上說明等主要功能提供豐富的腳本撰寫體驗。

## <a name="using-powershellexe"></a>使用 PowerShell.exe

**PowerShell_ISE.exe**工具會啟動 Windows PowerShell ISE 會話。 當您使用**PowerShell_ISE.exe**時，您可以使用它的選擇性參數，在 Windows PowerShell ISE 中開啟檔案，或啟動不含設定檔或多執行緒單元的 Windows PowerShell ISE 會話。

- 若要在 [命令提示字元] 視窗中啟動 Windows PowerShell ISE 會話，請在 Windows PowerShell 中，或在 [**開始**] 功能表上輸入：

  ```powershell
  PowerShell_Ise.exe
  ```

- 若要在 Windows PowerShell ISE 中開啟腳本（ps1）、腳本模組（. .psm1）、模組資訊清單（.psd1）、XML 檔案或任何其他支援的檔案，請輸入：

  ```powershell
  PowerShell_Ise.exe <filepath>
  ```

  在 Windows PowerShell 3.0 中，您可以使用選擇性的**File**參數，如下所示：

  ```powershell
  PowerShell_Ise.exe -file <filepath>
  ```

- 若要在沒有 Windows PowerShell 設定檔的情況下啟動 Windows PowerShell ISE 會話，請使用**NoProfile**參數。 （ **NoProfile**參數是在 Windows PowerShell 3.0 中引進），請輸入：

  ```powershell
  PowerShell_Ise.exe -NoProfile
  ```

- 若要查看 PowerShell_ISE.exe 說明檔，請輸入：

    ```powershell
    PowerShell_Ise.exe -help
    PowerShell_Ise.exe -?
    PowerShell_Ise.exe /?
    ```

### <a name="remarks"></a>備註

- 如需**PowerShell_ISE.exe**命令列參數的完整清單，請參閱[about_PowerShell_Ise.Exe](/powershell/module/microsoft.powershell.core/about/about_powershell_ise_exe)。

- 如需啟動 Windows PowerShell 的其他方式的詳細資訊，請參閱[啟動 Windows powershell](/powershell/scripting/windows-powershell/starting-windows-powershell)。

- Windows PowerShell 會在 Windows Server 作業系統的 Server Core 安裝選項上執行。 不過，由於 Windows PowerShell ISE 需要圖形使用者介面，因此不會在 Server Core 安裝上執行。

## <a name="additional-references"></a>其他參考

- [about_PowerShell_Ise.exe](/powershell/module/microsoft.powershell.core/about/about_powershell_exe)
