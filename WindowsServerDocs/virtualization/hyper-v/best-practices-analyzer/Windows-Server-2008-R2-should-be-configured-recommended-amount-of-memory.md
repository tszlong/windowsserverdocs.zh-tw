---
title: Windows Server 2008 R2 應設定為建議的記憶體數量
description: 瞭解當執行 Windows Server 2008 R2 的虛擬機器設定為小於建議的 RAM 數量（也就是 2 GB）時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 26872519-ccf0-4757-827f-8df2a7a2b9f9
ms.date: 8/16/2016
ms.openlocfilehash: 183f09c40a3d62afc46cc72e12de20adb2b231a4
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834213"
---
# <a name="windows-server-2008-r2-should-be-configured-with-the-recommended-amount-of-memory"></a>Windows Server 2008 R2 應設定為建議的記憶體數量

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*執行 Windows Server 2008 R2 的虛擬機器設定為小於建議的 RAM 數量，也就是 2 GB。*

## <a name="impact"></a>影響

*客體作業系統和應用程式可能無法正常執行。可能沒有足夠的記憶體可同時執行多個應用程式。這會影響下列虛擬機器：*

\<list of virtual machine names>

## <a name="resolution"></a>解決方案

*使用 Hyper-v 管理員將配置給此虛擬機器的記憶體增加到至少 2 GB。*

### <a name="to-increase-the-memory-using-hyper-v-manager"></a>使用 Hyper-v 管理員來增加記憶體

1.  開啟 Hyper-V 管理員。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。

2.  在結果窗格的 [ **虛擬機器**] 下，選取您要設定的虛擬機器。 虛擬機器的狀態應列為 [ **關閉**]。 如果不是，請在虛擬機器上按一下滑鼠右鍵，然後按一下 [ **關機**]。

3.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。

4.  在流覽窗格中，按一下 [ **記憶體**]。

5.  在 [ **記憶體** ] 頁面上，將 [ **啟動 RAM** ] 設定為至少 2 GB，然後按一下 **[確定]**。

### <a name="increase-the-memory-using-windows-powershell"></a>使用 Windows PowerShell 來增加記憶體

1.  開啟 Windows PowerShell。 從桌面 (中，按一下 [ **開始** ]，然後開始輸入 **Windows PowerShell**。 ) 

2.  以滑鼠右鍵按一下 **Windows PowerShell** ，然後按一下 [以 **系統管理員身分執行**]。

3.  \<MyVM>以您的虛擬機器名稱取代之後，執行此命令：

```
Set-VMMemory <MyVM> -StartupBytes 2GB
```

## <a name="see-also"></a>另請參閱
[設定-Get-vmmemory](/powershell/module/hyper-v/set-vmmemory)
