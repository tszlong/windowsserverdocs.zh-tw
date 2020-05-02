---
title: PowerShell_ise
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fb143c3d365b47f66aee5c64bfdc7dc26e5794f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723286"
---
# <a name="powershell_ise"></a>PowerShell_ise



Windows PowerShell 整合式腳本環境（ISE）是一個圖形化主機應用程式，可讓您在圖形輔助環境中讀取、寫入、執行、偵測及測試腳本和模組。 IntelliSense、顯示命令、程式碼片段、tab 鍵自動完成、語法著色、視覺化調試和即時線上說明等主要功能提供豐富的腳本撰寫體驗。

**PowerShell_ISE .exe**工具會啟動 Windows PowerShell ISE 會話。 當您使用**PowerShell_ISE .exe**時，您可以使用它的選擇性參數，在 Windows PowerShell ISE 中開啟檔案，或是啟動不含設定檔或多執行緒單元的 Windows PowerShell ISE 會話。

**PowerShell_ISE**是在 windows powershell 2.0 引進，並在 windows powershell 3.0 中大幅擴充。

## <a name="using-powershell_iseexe"></a>使用 PowerShell_ISE .exe

您可以使用**PowerShell_ISE**來啟動和結束 Windows PowerShell 會話，如下所示：
- 若要啟動 Windows PowerShell ISE 會話，請在 [命令提示字元] 視窗的 Windows PowerShell 中，或在 [開始] 功能表中輸入：  
  ```
  PowerShell_Ise
  ```  
- 若要在 Windows PowerShell ISE 中開啟腳本（ps1）、腳本模組（. .psm1）、模組資訊清單（.psd1）、XML 檔案或任何其他支援的檔案，請使用下列命令格式：  
  ```
  PowerShell_Ise <FilePath>
  ```  
  在 Windows PowerShell 3.0 中，您可以使用選擇性的**File**參數，如下所示：  
  ```
  PowerShell_Ise -File <FilePath>
  ```  
- 若要在沒有 Windows PowerShell 設定檔的情況下啟動 Windows PowerShell ISE 會話，請使用**NoProfile**參數。 （ **NoProfile**參數是在 Windows PowerShell 3.0 引進）。  
  ```
  PowerShell_Ise -NoProfile
  ```  
- 若要在 [命令提示字元] 視窗中查看**PowerShell_ISE .exe**說明檔，請使用下列命令格式：  
  ```
  PowerShell_Ise -help, -?, /?
  ```  
  如需**PowerShell_ISE .exe**命令列參數的完整清單，請參閱[about_PowerShell_Ise .exe](https://go.microsoft.com/fwlink/?LinkId=256512)。

## <a name="start-windows-powershell-ise-in-other-ways"></a>以其他方式啟動 Windows PowerShell ISE

如需其他啟動 Windows PowerShell ISE 方式的相關資訊，請參閱[啟動 Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259)。

## <a name="remarks"></a>備註

Windows PowerShell 會在 Windows Server 作業系統的 Server Core 安裝選項上執行。 不過，由於 Windows PowerShell ISE 需要圖形使用者介面，因此不會在 Server Core 安裝上執行。

## <a name="additional-references"></a>其他參考資料

[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512) 
[使用 windows powershell 的](https://technet.microsoft.com/scriptcenter/dd742419)about_PowerShell_Ise .exe[about_PowerShell .exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[windows powershell](https://go.microsoft.com/fwlink/?LinkID=107116)
腳本，另請參閱