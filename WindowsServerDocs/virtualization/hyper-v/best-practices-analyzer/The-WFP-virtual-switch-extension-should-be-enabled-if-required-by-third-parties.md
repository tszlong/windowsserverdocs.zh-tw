---
title: 應該啟用協力廠商擴充功能所需的 WFP 虛擬交換器擴充功能
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5afe706c246276597b32400109370ba3129e5a24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850619"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>應該啟用協力廠商擴充功能所需的 WFP 虛擬交換器擴充功能

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*Windows 篩選平台 (WFP) 的虛擬交換器擴充功能已停用。*  
  
## <a name="impact"></a>**影響**  
*在下列虛擬交換器上，有些協力廠商虛擬交換器擴充功能可能無法正常運作：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*使用 Windows PowerShell cmdlet，Enable-vmswitchextension，如果協力廠商擴充功能需要啟用 Windows 篩選平台。*  
  
### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>啟用 Windows 篩選平台使用 Windows PowerShell  
  
1.  開啟 Windows PowerShell。 (從桌面上，按一下**開始**，並開始輸入**Windows PowerShell**。)  
  
2.  以滑鼠右鍵按一下**Windows PowerShell**然後按一下**系統管理員身分執行**。  
  
3.  以您的外部交換器的名稱取代外部之後執行此命令：  
  
```  
Enable-VMSwitchExtension -VMSwitchName External -Name "Microsoft Windows Filtering Platform"  
```  
  
## <a name="see-also"></a>另請參閱  
[Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx)  
  


