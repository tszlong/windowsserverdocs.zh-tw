---
title: 應該至少以最小記憶體數量設定 Windows 8。1
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 84d7edab-610e-4265-87d0-9869f64b0039
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d8524039d23b0f5df523218177ba21568be27b54
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855531"
---
# <a name="windows-81-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>應該至少以最小記憶體數量設定 Windows 8。1

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|錯誤|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。
  
## <a name="issue"></a>**問題**  
*執行 Windows 8.1 的虛擬機器所設定的 RAM 小於最小容量，也就是 512 MB。*  
  
## <a name="impact"></a>**產生**  
*下列虛擬機器上的客體作業系統可能無法執行，或可能執行 unreliably：*  
```  
<list of virtual machines>  
```  
## <a name="resolution"></a>**解決方法**  
*使用 [Hyper-v 管理員]，將配置給此虛擬機器的記憶體增加到至少 512 MB。*  
  
#### <a name="increase-the-memory-using-hyper-v-manager"></a>使用 Hyper-v 管理員增加記憶體  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  在 [結果] 窗格的 [**虛擬機器**] 底下，選取您要設定的虛擬機器。 虛擬機器的狀態應列為 [**關閉**]。 如果不是，請在虛擬機器上按一下滑鼠右鍵，然後按一下 [**關機**]。  
  
3.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。  
  
4.  在流覽窗格中，按一下 [**記憶體**]。  
  
5.  在 [**記憶體**] 頁面上，將 [**啟動 RAM** ] 設定為至少 512 MB，然後按一下 **[確定]** 。  
  
### <a name="increase-the-memory-using-windows-powershell"></a>使用 Windows PowerShell 增加記憶體  
  
1.  開啟 Windows PowerShell。 （從桌面上，按一下 [**開始**]，然後開始鍵入**Windows PowerShell**。）  
  
2.  以滑鼠右鍵按一下 [ **Windows PowerShell** ]，然後按一下 [**以系統管理員身分執行**]。  
  
3.  以虛擬機器的名稱取代 \<MyVM > 之後，請執行此命令：  
  
```  
Set-VMMemory <MyVM> -StartupBytes 512MB  
```  
  
## <a name="see-also"></a>另請參閱  
[設定-Set-vmmemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


