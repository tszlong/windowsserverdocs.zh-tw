---
title: 應該使用建議的記憶體數量來設定 Windows Server 2008
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a98a8594-603b-487a-8739-78887c568e57
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: a563f809d067345ad6a17dbfaed052ab1010b2b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364376"
---
# <a name="windows-server-2008-should-be-configured-with-the-recommended-amount-of-memory"></a>應該使用建議的記憶體數量來設定 Windows Server 2008

>適用於︰Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|設定|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*執行 Windows Server 2008 的虛擬機器設定了小於建議的 RAM 容量，也就是 2 GB。*  
  
## <a name="impact"></a>影響  
  
*客體作業系統和應用程式可能無法順利執行。可能沒有足夠的記憶體可以同時執行多個應用程式。這會影響下列虛擬機器：*  
   
\<虛擬機器名稱清單 >  
  
## <a name="resolution"></a>解析度  
  
*使用 [Hyper-v 管理員]，將配置給此虛擬機器的記憶體增加至少 2 GB。*  
  
### <a name="increase-the-memory-using-hyper-v-manager"></a>使用 Hyper-v 管理員增加記憶體  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  在 [結果] 窗格的 [**虛擬機器**] 底下，選取您要設定的虛擬機器。 虛擬機器的狀態應列為 [**關閉**]。 如果不是，請在虛擬機器上按一下滑鼠右鍵，然後按一下 [**關機**]。  
  
3.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。  
  
4.  在流覽窗格中，按一下 [**記憶體**]。  
  
5.  在 [**記憶體**] 頁面上，將 [**啟動 RAM** ] 設定為至少 2 GB，然後按一下 **[確定]** 。  
  
### <a name="increase-memory-using-windows-powershell"></a>使用 Windows PowerShell 增加記憶體  
  
1.  開啟 Windows PowerShell。 （從桌面上，按一下 [**開始**]，然後開始鍵入**Windows PowerShell**。）  
  
2.  以滑鼠右鍵按一下 [ **Windows PowerShell** ]，然後按一下 [**以系統管理員身分執行**]。  
  
3.  執行類似下列的命令，以您的虛擬機器名稱取代 \<MyVM >，並以至少如下所示的值取代記憶體值。  
  
```  
Set-VMMemory <MyVM> -StartupBytes 2GB  
```  
  
## <a name="see-also"></a>請參閱  
[設定-Set-vmmemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


