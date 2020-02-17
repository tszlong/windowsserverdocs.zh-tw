---
title: Use local resources on Hyper-V virtual machine with VMConnect
description: 說明使用本機資源搭配 VMConnect 的需求
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18eface5-7518-4c6b-9282-93e2e3e87492
author: KBDAzure
ms.author: kathyDav
ms.date: 12/06/2016
ms.openlocfilehash: 70bf72ec2277679820d985c9f78f10a4ea6e04df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392893"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Use local resources on Hyper-V virtual machine with VMConnect

>適用於：Windows 10、Windows 8.1、Windows Server 2016 和 Windows Server 2012 R2

使用虛擬機器連線 (VMConnect) 時，您可以使用虛擬機器中的電腦本機資源，例如卸除式 USB 快閃磁碟機或印表機。 加強的工作階段模式也可讓您重新調整 VMConnect 視窗的大小。 本文說明如何設定主機，然後將本機資源的存取權授與虛擬機器。

加強的工作階段模式和輸入剪貼簿文字僅適用於執行最近 Windows 作業系統的虛擬機器。 \(請參閱以下的[使用本機資源的需求](#requirements-for-using-local-resources)。\) 

如需執行 Ubuntu 的虛擬機器詳細資訊，請參閱[變更 Hyper-V VM 中的 Ubuntu 螢幕解析度](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/)。 
  
## <a name="turn-on-enhanced-session-mode-on-a-hyper-v-host"></a>在 Hyper-V 主機中開啟加強的工作階段  
如果您的 Hyper-V 主機執行 Windows 10 或 Windows 8.1，則預設會開啟加強的工作階段模式，因此您可以略過此動作並移至下一節。 但是，如果您的主機執行 Windows Server 2016 或 Windows Server 2012 R2，請先執行此動作。 
  
開啟加強的工作階段模式：

1.  連線到裝載虛擬機器的電腦。  
  
2.  在 Hyper-V 管理員中，選取主機的電腦名稱。  
  
    ![顯示列在左側窗格中 [Hyper-V 管理員] 下的主機電腦名稱螢幕擷取畫面。](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  選取 [Hyper-V 設定]  。  
  
    ![顯示右側窗格中 [動作] 下的 [Hyper-V 設定] 選項螢幕擷取畫面。](media/HyperV-ActionsHyperVSettings.png)  
  
4.  在 [伺服器]  下，選取 [加強的工作階段模式原則]  。  
  
    ![顯示 [安全性] 區段下 [加強的工作階段模式原則] 選項的螢幕擷取畫面。](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  選取 [允許加強的工作階段模式]  核取方塊。  
  
    ![顯示 [加強的工作階段模式原則] 的 [允許加強的工作階段模式] 核取方塊螢幕擷取畫面。](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  在 [使用者]  下，選取 [加強的工作階段模式]  。  
  
    ![顯示 [使用者] 區段下 [加強的工作階段模式] 選項的螢幕擷取畫面。 ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  選取 [允許加強的工作階段模式]  核取方塊。  
  
8.  按一下 [確定]  。  
  
## <a name="choose-a-local-resource"></a>選擇本機資源

本機資源包括印表機、剪貼簿，以及執行 VMConnect 之電腦上的本機磁碟機。 如需詳細資訊，請參閱以下的[使用本機資源的需求](#requirements-for-using-local-resources)。  
  
要選擇本機資源：
  
1.  開啟 VMConnect。  
  
2.  選取您想要連線的虛擬機器。  
  
3.  按一下 [顯示選項]  。  
  
    ![呼叫對話方塊左下方的 [顯示] 選項螢幕擷取畫面。](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  選取 [本機資源]  。  
  
    ![呼叫 [本機資源] 索引標籤的螢幕擷取畫面。](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  按一下 [其他]  。  
  
    ![呼叫 [更多] 按鈕的螢幕擷取畫面。](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  選取您想要在虛擬機器上使用的磁碟機，然後按一下 [確定]  。  
  
    ![顯示您可以選取的本機資源和磁碟機的螢幕擷取畫面。](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  選取 [儲存我的設定，供將來連線到這個虛擬機器]  。  
  
    ![呼叫此選項可選取之核取方塊的螢幕擷取畫面。](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  按一下 [連線]  。  
  
## <a name="edit-vmconnect-settings"></a>編輯 VMConnect 設定

您可以在 Windows PowerShell 或命令提示字元中執行下列命令，輕鬆編輯您的 VMConnect 連線設定：  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
## <a name="requirements-for-using-local-resources"></a>使用本機資源的需求

若要能夠在虛擬機器上使用電腦的本機資源：  
  
-   Hyper-V 主機必須開啟 [加強的工作階段模式原則]  和 [加強的工作階段模式]  設定。  
  
-   您使用 VMConnect 的電腦必須執行 Windows 10、Windows 8.1、Windows Server 2016 或 Windows Server 2012 R2。  
  
-   虛擬機器必須已啟用遠端桌面服務，並執行 Windows 10、Windows 8.1、Windows Server 2016 或 Windows Server 2012 R2 做為客體作業系統。  
  
如果執行 VMConnect 和虛擬機器的電腦都符合需求，您可以使用下列任何可用的本機資源：  
  
-   顯示器設定  
  
-   音訊
  
-   印表機  
  
-   可供複製及貼上的剪貼簿  
  
-   智慧卡  
  
-   USB 裝置  
  
-   磁碟機  
  
-   支援的隨插即用裝置  
  
## <a name="why-use-a-computers-local-resources"></a>為何要使用電腦的本機資源？
建議您使用電腦的本機資源來進行下列作業：  
  
-   在沒有可連到虛擬機器的網路連線情況下，對虛擬機器進行疑難排解。  
  
-   以與使用「遠端桌面連線」(RDP) 相同的方式，將檔案複製及貼到虛擬機器或從該處複製及貼上。  
  
-   使用智慧卡來登入虛擬機器。  
  
-   從虛擬機器列印到本機印表機。  
  
-   在不使用 RDP 的情況下，對需要 USB 和健全重新導向的開發人員應用程式進行測試和疑難排解。  
  
## <a name="see-also"></a>另請參閱  
[與虛擬機器連線](https://technet.microsoft.com/library/cc742407.aspx)  
[我應在 Hyper-V 建立第 1 或 2 代的虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)



