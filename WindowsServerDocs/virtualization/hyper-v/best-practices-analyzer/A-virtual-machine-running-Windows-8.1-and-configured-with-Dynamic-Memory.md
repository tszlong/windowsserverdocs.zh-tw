---
title: 執行 Windows 8.1 並以動態記憶體設定的虛擬機器應該使用記憶體設定的建議值
description: 提供指示以解決這個最佳做法分析程式規則所報告的問題。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: b9a14f85-326f-4916-9278-2c8d39a32848
ms.date: 8/16/2016
ms.openlocfilehash: 7ad74e982ad75a7763c37ad28599642c593b5612
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746333"
---
# <a name="a-virtual-machine-running-windows-81-and-configured-with-dynamic-memory-should-use-recommended-values-for-memory-settings"></a>執行 Windows 8.1 並以動態記憶體設定的虛擬機器應該使用記憶體設定的建議值

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
*一或多部虛擬機器已設定為使用動態記憶體，其小於建議用於 Windows 8.1 的記憶體數量。*

## <a name="impact"></a>**影響**
下列虛擬機器上的客體作業系統可能無法執行，或可能執行 unreliably：

\<list of virtual machines>


## <a name="resolution"></a>**解決方法**
*您可以使用 Hyper-v 管理員，將至少 256 MB 的最小記憶體、啟動記憶體增加到至少 512 MB，並將最大記憶體增加到此虛擬機器至少 1 GB。*

#### <a name="increase-memory-using-hyper-v-manager"></a>使用 Hyper-v 管理員來增加記憶體

1.  開啟 Hyper-V 管理員。 從伺服器管理員 (，請按一下 [**工具**  >  **hyper-v 管理員**]。 ) 

2.  從虛擬機器清單中，以滑鼠右鍵按一下您想要的虛擬機器，然後按一下 [ **設定**]。

3.  在流覽窗格中，按一下 [ **記憶體**]。

4.  將 **RAM** 變更為至少 512 MB。

5.  在 [ **動態記憶體**] 下，將 [ **最小 ram** ] 變更為至少 256 MB， **最大 ram** 變更為 1 GB。

6.  按一下 [確定]。

### <a name="increase-memory-using-windows-powershell"></a>使用 Windows PowerShell 來增加記憶體

1.  開啟 Windows PowerShell。 從桌面 (中，按一下 [開始]，然後開始輸入 **Windows PowerShell**。 ) 

2.  以滑鼠右鍵按一下 **Windows PowerShell** ，然後按一下 [以 **系統管理員身分執行**]。

3.  執行類似下列的命令，將 MyVM 取代為您虛擬機器的名稱，並以至少如下所示的值來取代記憶體值。

```
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 1GB -MinimumBytes 256MB -StartupBytes 512MB
```



