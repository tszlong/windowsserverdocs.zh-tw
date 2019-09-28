---
title: 執行 Windows Server 2012 並設定動態記憶體的虛擬機器應該使用建議的記憶體設定值
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 0aa35e36-8e3b-498b-b71d-003a0a0947be
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4eb223fa2638e453bae14bdbae5029ae5835207e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365349"
---
# <a name="a-virtual-machine-running-windows-server-2012-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>執行 Windows Server 2012 並設定動態記憶體的虛擬機器應該使用建議的記憶體設定值

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*一或多部虛擬機器已設定為使用動態記憶體，且小於建議用於 Windows Server 2012 的記憶體數量。*  
  
## <a name="impact"></a>**產生**  
*下列虛擬機器上的客體作業系統可能無法執行，或可能執行 unreliably：*  
  
@no__t 0list 的虛擬機器 >  
  
## <a name="resolution"></a>**解決方法**  
*使用 Hyper-v 管理員將最小值增加到至少 256 MB，啟動記憶體至少為 512 MB，最大記憶體為至少 2 GB （針對此虛擬機器）。*  
  
#### <a name="increase-memory-using-hyper-v-manager"></a>使用 Hyper-v 管理員增加記憶體  
  
1.  開啟 \[Hyper-V 管理員\]。 （從伺服器管理員，按一下 [**工具**] [ > ] [**hyper-v 管理員**]）。  
  
2.  從虛擬機器清單中，以滑鼠右鍵按一下您想要的，然後按一下 [**設定**]。  
  
3.  在流覽窗格中，按一下 [**記憶體**]。  
  
4.  請將**RAM**變更為至少 512 MB。  
  
5.  在 [**動態記憶體**] 底下，將 [**最小 ram** ] 變更為至少 256 MB，並將 [**最大 RAM** ] 設為 2 GB  
  
6.  按一下 [確定]。  
  
### <a name="increase-memory-using-windows-powershell"></a>使用 Windows PowerShell 增加記憶體  
  
1.  開啟 Windows PowerShell。 （從桌面上，按一下 [**開始**]，然後開始鍵入**Windows PowerShell**。）  
  
2.  以滑鼠右鍵按一下 [ **Windows PowerShell** ]，然後按一下 [**以系統管理員身分執行**]。  
  
3.  執行類似下列的命令，以您的虛擬機器名稱取代 MyVM，並以至少如下所示的值取代記憶體值。  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 2GB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


