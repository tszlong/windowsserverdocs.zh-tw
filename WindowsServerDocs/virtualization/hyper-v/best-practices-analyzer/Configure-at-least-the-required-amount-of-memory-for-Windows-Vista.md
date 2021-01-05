---
title: 至少為執行 Windows Vista 且啟用動態記憶體的虛擬機器設定所需的記憶體數量
description: 瞭解當一或多部虛擬機器設定為使用小於 Windows Vista 所需記憶體數量的動態記憶體時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: d3de7614-6eee-4839-a939-d390bca9ba89
ms.date: 8/16/2016
ms.openlocfilehash: d18ab7890e2043db131de37f1f3cff1b24985237
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834303"
---
# <a name="configure-at-least-the-required-amount-of-memory-for-a-virtual-machine-running-windows-vista-and-enabled-for-dynamic-memory"></a>至少為執行 Windows Vista 且啟用動態記憶體的虛擬機器設定所需的記憶體數量

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*一或多部虛擬機器設定為使用的動態記憶體，少於 Windows Vista 所需的記憶體數量。*

## <a name="impact"></a>影響
*下列虛擬機器上的客體作業系統可能無法執行，或可能執行 unreliably：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*使用 [Hyper-v 管理員] 或 Windows PowerShell，將最小記憶體增加到至少 256 MB，並將 [啟動記憶體] 和 [最大記憶體] 增加到至少 512 MB。*

### <a name="increase-memory-using-hyper-v-manager"></a>使用 Hyper-v 管理員來增加記憶體

1.  開啟 Hyper-V 管理員。 從伺服器管理員 (，請按一下 [**工具**  >  **hyper-v 管理員**]。 ) 

2.  從虛擬機器清單中，以滑鼠右鍵按一下您想要的虛擬機器，然後按一下 [ **設定**]。

3.  在流覽窗格中，按一下 [ **記憶體**]。

4.  將 **RAM** 變更為至少 512 MB。

5.  在 [ **動態記憶體**] 下，將 **最小 Ram 的最小值** 變更為至少 256 MB， **最大 ram** 變更為 512 MB。

6.  按一下 [確定]。

### <a name="increase-memory-using-windows-powershell"></a>使用 Windows PowerShell 來增加記憶體

1.  開啟 Windows PowerShell。 從桌面 (中，按一下 [ **開始** ]，然後開始輸入 **Windows PowerShell**。 ) 

2.  以滑鼠右鍵按一下 **Windows PowerShell** ，然後按一下 [以 **系統管理員身分執行**]。

3.  執行類似下列的命令，將 MyVM 取代為您虛擬機器的名稱，並以至少如下所示的值來取代記憶體值。

```
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 512MB -MinimumBytes 256MB -StartupBytes 512MB
```



