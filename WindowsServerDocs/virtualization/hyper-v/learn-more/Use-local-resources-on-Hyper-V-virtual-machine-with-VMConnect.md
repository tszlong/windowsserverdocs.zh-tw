---
title: Use local resources on Hyper-V virtual machine with VMConnect
description: 說明搭配 VMConnect 使用本機資源的需求
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
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392893"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Use local resources on Hyper-V virtual machine with VMConnect

>適用于： Windows 10、Windows 8.1、Windows Server 2016、Windows Server 2012 R2

虛擬機器連線（VMConnect）可讓您在虛擬機器（例如卸載式 USB 快閃磁片磁碟機或印表機）中使用電腦的本機資源。 增強的會話模式也可讓您調整 VMConnect 視窗的大小。 本文說明如何設定主機，然後將本機資源的存取權授與虛擬機器。

[增強的會話模式] 和 [類型剪貼簿] 文字僅適用于執行最近的 Windows 作業系統的虛擬機器。 \(參閱下面[的使用本機資源的需求](#requirements-for-using-local-resources)。\) 

如需執行 Ubuntu 的虛擬機器，請參閱[變更 HYPER-V VM 中的 Ubuntu 螢幕解析度](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/)。 
  
## <a name="turn-on-enhanced-session-mode-on-a-hyper-v-host"></a>在 Hyper-v 主機上開啟增強的會話模式  
如果您的 Hyper-v 主機執行 Windows 10 或 Windows 8.1，預設會開啟增強的會話模式，因此您可以略過此動作並移至下一節。 但是，如果您的主機執行 Windows Server 2016 或 Windows Server 2012 R2，請先執行此動作。 
  
開啟增強的會話模式：

1.  連線到裝載虛擬機器的電腦。  
  
2.  在 [Hyper-v 管理員] 中，選取主機的電腦名稱稱。  
  
    ![螢幕擷取畫面：顯示列在左窗格中 [Hyper-v 管理員] 底下的主電腦名稱稱。](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  選取 [Hyper-V 設定]。  
  
    ![螢幕擷取畫面，顯示右窗格中 [動作] 底下的 [Hyper-v 設定] 選項。](media/HyperV-ActionsHyperVSettings.png)  
  
4.  在 [伺服器] 下，選取 [加強的工作階段模式原則]。  
  
    ![顯示 [安全性] 區段下 [增強的會話模式] 原則選項的螢幕擷取畫面。](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  選取 [允許加強的工作階段模式] 核取方塊。  
  
    ![顯示增強的會話模式原則之 [允許增強的會話模式] 核取方塊的螢幕擷取畫面。](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  在 [使用者]下，選取 [加強的工作階段模式]。  
  
    ![螢幕擷取畫面：顯示 [使用者] 區段下的 [增強的會話模式] 選項。 ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  選取 [允許加強的工作階段模式] 核取方塊。  
  
8.  按一下 [確定]。  
  
## <a name="choose-a-local-resource"></a>選擇本機資源

本機資源包括印表機、剪貼簿，以及您執行 VMConnect 之電腦上的本機磁片磁碟機。 如需詳細資訊，請參閱下方的[使用本機資源的需求](#requirements-for-using-local-resources)。  
  
若要選擇本機資源：
  
1.  開啟 VMConnect。  
  
2.  選取您想要連線的虛擬機器。  
  
3.  按一下 [顯示選項]。  
  
    ![在對話方塊左下方呼叫 [顯示選項] 的螢幕擷取畫面。](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  選取 [本機資源]。  
  
    ![呼叫 [本機資源] 索引標籤的螢幕擷取畫面。](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  按一下 [其他]。  
  
    ![呼叫 [更多] 按鈕的螢幕擷取畫面。](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  選取您想要在虛擬機器上使用的磁碟機，然後按一下 [確定]。  
  
    ![螢幕擷取畫面：顯示您可以選取的本機資源和磁片磁碟機。](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  選取 [儲存我的設定，供將來連線到這個虛擬機器]。  
  
    ![呼叫核取方塊以選取此選項的螢幕擷取畫面。](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  按一下 [連線]。  
  
## <a name="edit-vmconnect-settings"></a>編輯 VMConnect 設定

您可以在 Windows PowerShell 或命令提示字元中執行下列命令，輕鬆編輯您的 VMConnect 連線設定：  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
## <a name="requirements-for-using-local-resources"></a>使用本機資源的需求

若要能夠在虛擬機器上使用電腦的本機資源：  
  
-   Hyper-v 主機必須開啟 [增強的**會話模式原則]** 和 [**增強的會話模式]** 設定。  
  
-   您使用 VMConnect 的電腦必須執行 Windows 10、Windows 8.1、Windows Server 2016 或 Windows Server 2012 R2。  
  
-   虛擬機器必須啟用遠端桌面服務，並執行 Windows 10、Windows 8.1、Windows Server 2016 或 Windows Server 2012 R2 作為客體作業系統。  
  
如果執行 VMConnect 和虛擬機器的電腦都符合需求，您可以使用下列任何本機資源（如果有的話）：  
  
-   顯示器設定  
  
-   音訊
  
-   印表機  
  
-   可供複製及貼上的剪貼簿  
  
-   智慧卡  
  
-   USB 裝置  
  
-   磁碟機  
  
-   支援的隨插即用裝置  
  
## <a name="why-use-a-computers-local-resources"></a>為何要使用電腦的本機資源？
您可能會想要使用電腦的本機資源來執行下列動作：  
  
-   在沒有可連到虛擬機器的網路連線情況下，對虛擬機器進行疑難排解。  
  
-   以與使用「遠端桌面連線」(RDP) 相同的方式，將檔案複製及貼到虛擬機器或從該處複製及貼上。  
  
-   使用智慧卡來登入虛擬機器。  
  
-   從虛擬機器列印到本機印表機。  
  
-   在不使用 RDP 的情況下，對需要 USB 和健全重新導向的開發人員應用程式進行測試和疑難排解。  
  
## <a name="see-also"></a>請參閱  
[連接至虛擬機器](https://technet.microsoft.com/library/cc742407.aspx)  
[我應該在 Hyper-v 中建立第1代或第2代虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)



