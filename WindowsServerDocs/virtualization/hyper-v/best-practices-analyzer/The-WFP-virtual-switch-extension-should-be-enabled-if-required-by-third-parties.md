---
title: 應該啟用協力廠商擴充功能所需的 WFP 虛擬交換器擴充功能
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
ms.date: 8/16/2016
ms.openlocfilehash: b6099410d4d5e043387594022e8bb376d5fb52f7
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746673"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>應該啟用協力廠商擴充功能所需的 WFP 虛擬交換器擴充功能

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*Windows 篩選平台 (WFP) 虛擬交換器擴充功能已停用。*

## <a name="impact"></a>**影響**
*某些協力廠商虛擬交換器擴充功能可能無法在下列虛擬交換器上正確運作：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方法**
*如果協力廠商延伸模組需要，請使用 Windows PowerShell Cmdlet Disable-vmswitchextension 來啟用 Windows 篩選平台。*

### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>使用 Windows PowerShell 啟用 Windows 篩選平台

1.  開啟 Windows PowerShell。 從桌面 (中，按一下 [ **開始** ]，然後開始輸入 **Windows PowerShell**。 ) 

2.  以滑鼠右鍵按一下 **Windows PowerShell** ，然後按一下 [以 **系統管理員身分執行**]。

3.  以外部參數的名稱取代 External 之後，執行此命令：

```
Enable-VMSwitchExtension -VMSwitchName External -Name Microsoft Windows Filtering Platform
```

## <a name="see-also"></a>另請參閱
[啟用-Disable-vmswitchextension](/powershell/module/hyper-v/enable-vmswitchextension?view=win10-ps)