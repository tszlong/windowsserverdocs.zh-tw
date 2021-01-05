---
title: 執行 Windows Server 2012 R2 並使用動態記憶體設定的虛擬機器應該使用記憶體設定的建議值
description: 瞭解當一或多部虛擬機器設定為使用小於 Windows Server 2012 R2 建議的記憶體數量動態記憶體時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 3a53c197-80ce-4b33-a83e-7e89e657a519
ms.date: 8/16/2016
ms.openlocfilehash: c3226ef03be3cfdb3e61742bc0aa09a642730105
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834923"
---
# <a name="a-virtual-machine-running-windows-server-2012-r2-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>執行 Windows Server 2012 R2 並使用動態記憶體設定的虛擬機器應該使用記憶體設定的建議值

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*一或多部虛擬機器設定為使用小於 Windows Server 2012 R2 建議的記憶體數量的動態記憶體。*

## <a name="impact"></a>影響
*下列虛擬機器上的客體作業系統可能無法執行，或可能執行 unreliably：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*您可以使用 Hyper-v 管理員，將至少 256 MB 的最小記憶體、啟動記憶體增加到至少 512 MB，並將最大記憶體增加到此虛擬機器至少 2 GB。*

#### <a name="increase-memory-using-hyper-v-manager"></a>使用 Hyper-v 管理員來增加記憶體

1.  開啟 Hyper-V 管理員。 從伺服器管理員 (，請按一下 [**工具**  >  **hyper-v 管理員**]。 ) 

2.  從虛擬機器清單中，以滑鼠右鍵按一下您想要的虛擬機器，然後按一下 [ **設定**]。

3.  在流覽窗格中，按一下 [ **記憶體**]。

4.  將 **RAM** 變更為至少 512 MB。

5.  在 [ **動態記憶體**] 下，將 **最低 ram** 變更為至少 256 MB，並將 **最大 ram** 變更為 2 GB。

6.  按一下 [確定]。

### <a name="increase-memory-using-windows-powershell"></a>使用 Windows PowerShell 來增加記憶體

1.  開啟 Windows PowerShell。 從桌面 (中，按一下 [ **開始** ]，然後開始輸入 **Windows PowerShell**。 ) 

2.  以滑鼠右鍵按一下 **Windows PowerShell** ，然後按一下 [以 **系統管理員身分執行**]。

3.  執行類似下列的命令，將 MyVM 取代為您虛擬機器的名稱，並以至少如下所示的值來取代記憶體值。

```
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 2GB -MinimumBytes 256MB -StartupBytes 512MB
```



