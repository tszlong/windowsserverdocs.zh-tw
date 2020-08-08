---
title: 應該使用建議的記憶體數量來設定 Windows Vista
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 64f4e53b-4adb-4e1d-bc48-c24f5f9d222f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: c1754283e8406944f45668d787f741d3fd573e1b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948157"
---
# <a name="windows-vista-should-be-configured-with-the-recommended-amount-of-memory"></a>應該使用建議的記憶體數量來設定 Windows Vista

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題

*執行 Windows Vista 的虛擬機器設定了小於建議的 RAM 數量，也就是 1 GB。*

## <a name="impact"></a>影響

*客體作業系統和應用程式可能無法順利執行。可能沒有足夠的記憶體可以同時執行多個應用程式。這會影響下列虛擬機器：*

\<list of virtual machine names>

## <a name="resolution"></a>解決方法

*使用 [Hyper-v 管理員]，將配置給此虛擬機器的記憶體增加到至少 1 GB。*

### <a name="to-increase-the-memory-allocated-to-a-virtual-machine"></a>增加配置給虛擬機器的記憶體

1.  開啟 Hyper-V 管理員。 按一下 [開始]****，指向 [系統管理工具]****，然後按一下 [Hyper-V 管理員]****。

2.  在 [結果] 窗格的 [**虛擬機器**] 底下，選取您要設定的虛擬機器。 虛擬機器的狀態應列為 [**關閉**]。 如果不是，請在虛擬機器上按一下滑鼠右鍵，然後按一下 [**關機**]。

3.  在 [執行]**** 窗格的虛擬機器名稱之下，按一下 [設定]****。

4.  在流覽窗格中，按一下 [**記憶體**]。

5.  在 [**記憶體**] 頁面上，將 [**啟動 RAM** ] 設定為至少 1 GB，然後按一下 **[確定]**。

### <a name="increase-the-memory-using-windows-powershell"></a>使用 Windows PowerShell 增加記憶體

1.  開啟 Windows PowerShell。 從桌面上 (，按一下 [**開始**]，然後開始鍵入**Windows PowerShell**。 ) 

2.  以滑鼠右鍵按一下 [ **Windows PowerShell** ]，然後按一下 [**以系統管理員身分執行**]。

3.  將取代 \<MyVM> 為您的虛擬機器名稱之後，請執行此命令：

```
Set-VMMemory <MyVM> -StartupBytes 1GB
```

## <a name="see-also"></a>另請參閱
[設定-Set-vmmemory](https://technet.microsoft.com/library/hh848572.aspx)



