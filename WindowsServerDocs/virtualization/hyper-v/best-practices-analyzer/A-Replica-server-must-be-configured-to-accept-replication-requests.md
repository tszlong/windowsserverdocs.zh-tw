---
title: 複本伺服器必須設定為接受複寫要求
description: 提供指示以解決這個最佳做法分析程式規則所報告的問題。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.date: 8/16/2016
ms.openlocfilehash: 941eccbafb7b84caf161f68b022c9a93fffd4f5f
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746383"
---
# <a name="a-replica-server-must-be-configured-to-accept-replication-requests"></a>複本伺服器必須設定為接受複寫要求

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*此電腦已指定為 Hyper-v 複本伺服器，但未設定為接受來自主伺服器的傳入複寫資料。*

## <a name="impact"></a>影響
*此伺服器無法接受來自主伺服器的複寫流量。*

## <a name="resolution"></a>解決方案
*使用 Hyper-v 管理員來指定此複本伺服器應接受複寫資料的主伺服器。*

#### <a name="create-authorization-entries-using-hyper-v-manager"></a>使用 Hyper-v 管理員建立授權專案

1.  開啟 Hyper-V 管理員。 從伺服器管理員 (，請按一下 [**工具**  >  **hyper-v 管理員**]。 ) 

2.  從主機清單中，以滑鼠右鍵按一下您要的主機，然後按一下 [ **Hyper-v 設定**]。

3.  在流覽窗格中 **，按一下 [** 複寫設定]。

4.  在 [ **授權與存放裝置**] 下，按一下 **[允許從指定的伺服器**複寫]。

5.  在伺服器清單底下，按一下 [ **新增**]。

6.  在 [ **新增授權專案**：

    -   輸入第一部伺服器的完整名稱。

    -   指定專用位置，只儲存該伺服器的檔案。

7.  按一下 [確定]。

8.  針對每一部主伺服器重複執行。

9. 再次按一下 **[確定** ] 以完成並關閉視窗。

### <a name="create-authorization-entries-using-windows-powershell"></a>使用 Windows PowerShell 建立授權專案

1.  開啟 Windows PowerShell。 從桌面 (中，按一下 [開始]，然後開始輸入 **Windows PowerShell**。 ) 

2.  以滑鼠右鍵按一下 **Windows PowerShell** ，然後按一下 [以 **系統管理員身分執行**]。

3.  執行類似下列的命令，並取代：

    -   Server01.domain01.contoso.com 的主伺服器名稱，以及伺服器的完整功能變數名稱。

    -   D:\ReplicaVMStorage 與您的位置的位置。

    -   名為 DEFAULT 的信任群組，如果您已建立，則為您群組的名稱。 如果沒有，請使用預設值。

```
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT
```



