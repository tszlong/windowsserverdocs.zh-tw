---
title: 複本伺服器必須設定為接受複寫要求
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 54868d4db2dccc893bd2897134d9125446873384
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366719"
---
# <a name="a-replica-server-must-be-configured-to-accept-replication-requests"></a>複本伺服器必須設定為接受複寫要求

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|Error|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。
  
## <a name="issue"></a>問題  
*這部電腦指定為 Hyper-v 複本伺服器，但未設定為接受來自主伺服器的傳入複寫資料。*  
  
## <a name="impact"></a>影響  
*這部伺服器無法接受來自主伺服器的複寫流量。*  
  
## <a name="resolution"></a>解析度  
*使用 [Hyper-v 管理員] 來指定此複本伺服器應接受複寫資料的主伺服器。*  
  
#### <a name="create-authorization-entries-using-hyper-v-manager"></a>使用 Hyper-v 管理員建立授權專案  
  
1.  開啟 \[Hyper-V 管理員\]。 （從伺服器管理員，按一下 [**工具**] [ > ] [**hyper-v 管理員**]）。  
  
2.  在主機清單中，以滑鼠右鍵按一下您想要的主控制項，然後按一下 [ **Hyper-v 設定**]。  
  
3.  在流覽窗格中 **，按一下 [** 複寫設定]。  
  
4.  在 [**授權與存放裝置**] 底下，按一下 **[允許從指定的伺服器進行**複寫]。  
  
5.  在伺服器清單下方，按一下 [**新增**]。  
  
6.  在 [**新增授權專案**] 底下：  
  
    -   輸入第一部伺服器的完整名稱。  
  
    -   指定專用的位置來儲存該伺服器的檔案。  
  
7.  按一下 [確定]。  
  
8.  針對每部主伺服器重複執行。  
  
9. 再按一次 **[確定**] 以完成並關閉視窗。  
  
### <a name="create-authorization-entries-using-windows-powershell"></a>使用 Windows PowerShell 建立授權專案  
  
1.  開啟 Windows PowerShell。 （從桌面上，按一下 [開始]，然後開始鍵入**Windows PowerShell**。）  
  
2.  以滑鼠右鍵按一下 [ **Windows PowerShell** ]，然後按一下 [**以系統管理員身分執行**]。  
  
3.  執行類似下列的命令，並取代：  
  
    -   Server01.domain01.contoso.com 的主伺服器名稱，具有伺服器的完整功能變數名稱。  
  
    -   D:\ReplicaVMStorage 與您的位置的位置。  
  
    -   名為 DEFAULT 的信任群組與您的群組名稱（如果您已建立）。 如果不是，請使用預設值。  
  
```  
New-VMReplicationAuthorizationEntry server01.domain01.contoso.com D:\ReplicaVMStorage DEFAULT  
```  
  


