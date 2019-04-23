---
title: 執行 Windows 8.1，並使用動態記憶體設定的虛擬機器應該用於記憶體設定的建議的值
description: 提供指示來解決此 Best Practices Analyzer 規則所回報的問題。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b9a14f85-326f-4916-9278-2c8d39a32848
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bb283b094791904e54f13efdc0b6848ecacc1a3a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859879"
---
# <a name="a-virtual-machine-running-windows-81-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>執行 Windows 8.1，並使用動態記憶體設定的虛擬機器應該用於記憶體設定的建議的值

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
*一或多個虛擬機器會設定為使用動態記憶體少於建議用於 Windows 8.1 的記憶體數量。*  
  
## <a name="impact"></a>**影響**  
客體作業系統，在下列虛擬機器可能無法執行，或可能 unreliably 執行：   
  
\<虛擬機器清單 >  
      
  
## <a name="resolution"></a>**解決方法**  
*您可以使用 HYPER-V 管理員來增加至少 256 MB，為至少 512 MB 的啟動記憶體和最大記憶體為此虛擬機器至少 1 GB 的記憶體下限。*  
  
#### <a name="increase-memory-using-hyper-v-manager"></a>使用 HYPER-V 管理員的增加記憶體  
  
1.  開啟 \[Hyper-V 管理員\]。 (從 [伺服器管理員] 中，按一下**工具** > **HYPER-V 管理員**。)  
  
2.  從清單中的虛擬機器，請以滑鼠右鍵按一下，然後按一下其中一個**設定**。  
  
3.  在 [導覽] 窗格中，按一下**記憶體**。  
  
4.  變更**RAM**為至少 512 MB。  
  
5.  底下**動態記憶體**，變更**最小 RAM**設為至少 256 MB， **RAM 上限**設為 1 GB。  
  
6.  按一下 [確定] 。  
  
### <a name="increase-memory-using-windows-powershell"></a>使用 Windows PowerShell 的增加記憶體  
  
1.  開啟 Windows PowerShell。 (從桌面上，按一下 [開始]，並開始輸入**Windows PowerShell**。)  
  
2.  以滑鼠右鍵按一下**Windows PowerShell**然後按一下**系統管理員身分執行**。  
  
3.  執行類似下列的命令，使用的記憶體與您的虛擬機器的名稱取代 MyVM 值與最少的值，如下所示。  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 1GB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


