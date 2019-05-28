---
title: Use local resources on Hyper-V virtual machine with VMConnect
description: 描述搭配 VMConnect 使用本機資源的需求
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18eface5-7518-4c6b-9282-93e2e3e87492
author: KBDAzure
ms.author: kathyDav
ms.date: 12/06/2016
ms.openlocfilehash: a7e465313c68ee793715aba045cc56a2ca5fd1de
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222845"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Use local resources on Hyper-V virtual machine with VMConnect

>適用於：Windows 10，Windows 8.1、 Windows Server 2016、windows Server 2012 R2

虛擬機器連線 (VMConnect) 可讓您在虛擬機器中，使用電腦的本機資源，例如卸除式 USB 快閃磁碟機或印表機。 增強的工作階段模式也可讓您調整 VMConnect 視窗的大小。 本文將說明如何設定主機，然後授與虛擬機器存取權的本機資源。

增強的工作階段模式與輸入剪貼簿文字是僅適用於執行新的 Windows 作業系統的虛擬機器。 \(請參閱[使用本機資源的需求](#requirements-for-using-local-resources)底下。\) 

對於執行 Ubuntu 的虛擬機器，請參閱[HYPER-V VM 中變更 Ubuntu 螢幕解析度](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/)。 
  
## <a name="turn-on-enhanced-session-mode-on-a-hyper-v-host"></a>開啟 HYPER-V 主機上的 增強的工作階段模式  
如果您的 HYPER-V 主機執行 Windows 10 或 Windows 8.1，增強的工作階段模式是在預設情況下，讓您可以略過這個並移至下一節。 但是，如果您的主機執行 Windows Server 2016 或 Windows Server 2012 R2，請先執行此作業。 
  
開啟加強的工作階段模式：

1.  連線到裝載虛擬機器的電腦。  
  
2.  在 HYPER-V 管理員 中，選取主機的電腦名稱。  
  
    ![螢幕擷取畫面顯示主機電腦名稱列在 HYPER-V 管理員下的左窗格中。](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  選取 [Hyper-V 設定]  。  
  
    ![如果螢幕擷取畫面顯示在右窗格中的 [動作] 底下的 [HYPER-V 設定] 選項。](media/HyperV-ActionsHyperVSettings.png)  
  
4.  在 [伺服器]  下，選取 [加強的工作階段模式原則]  。  
  
    ![如果螢幕擷取畫面顯示在 [安全性] 區段下的 [增強的工作階段模式] 原則選項。](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  選取 [允許加強的工作階段模式]  核取方塊。  
  
    ![螢幕擷取畫面顯示 允許增強的工作階段模式的增強型工作階段模式原則的核取方塊。](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  在 [使用者]  下，選取 [加強的工作階段模式]  。  
  
    ![如果螢幕擷取畫面顯示在 [使用者] 區段下的 [增強的工作階段模式] 選項。 ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  選取 [允許加強的工作階段模式]  核取方塊。  
  
8.  按一下 [確定]  。  
  
## <a name="choose-a-local-resource"></a>選擇 本機資源

本機資源包括印表機、 剪貼簿和您要在其中執行 VMConnect 之電腦上的本機磁碟機。 如需詳細資訊，請參閱 <<c0> [ 使用本機資源的需求](#requirements-for-using-local-resources)底下。  
  
若要選擇本機資源：
  
1.  開啟 VMConnect。  
  
2.  選取您想要連線的虛擬機器。  
  
3.  按一下 [顯示選項]  。  
  
    ![如果螢幕擷取畫面左下方的顯示選項 對話方塊的 出呼叫。](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  選取 [本機資源]  。  
  
    ![如果螢幕擷取畫面會呼叫外面的本機資源 索引標籤。](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  按一下 [其他]  。  
  
    ![螢幕擷取畫面的 [更多資訊] 按鈕會加以標注。](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  選取您想要在虛擬機器上使用的磁碟機，然後按一下 [確定]  。  
  
    ![如果螢幕擷取畫面顯示本機資源，您可以選取的磁碟機。](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  選取 [儲存我的設定，供將來連線到這個虛擬機器]  。  
  
    ![如果螢幕擷取畫面以選取此選項的核取方塊會加以標注。](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  按一下 **[連接]** 。  
  
## <a name="edit-vmconnect-settings"></a>編輯 VMConnect 設定

您可以在 Windows PowerShell 或命令提示字元中執行下列命令，輕鬆編輯您的 VMConnect 連線設定：  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
## <a name="requirements-for-using-local-resources"></a>使用本機資源的需求

若要能夠使用電腦的本機資源的虛擬機器上：  
  
-   HYPER-V 主機必須具有**加強的工作階段模式原則**並**加強的工作階段模式**開啟設定。  
  
-   您可以在其使用 VMConnect 的電腦必須執行 Windows 10，Windows 8.1、 Windows Server 2016 或 Windows Server 2012 R2。  
  
-   虛擬機器必須啟用，並執行 Windows 10，Windows 8.1、 Windows Server 2016 或 Windows Server 2012 R2 作為客體作業系統的遠端桌面服務。  
  
如果執行 VMConnect 和虛擬機器的電腦都符合需求，您可以使用任何下列本機資源，如果它們可用：  
  
-   顯示器設定  
  
-   音訊
  
-   印表機  
  
-   可供複製及貼上的剪貼簿  
  
-   智慧卡  
  
-   USB 裝置  
  
-   磁碟機  
  
-   支援的隨插即用裝置  
  
## <a name="why-use-a-computers-local-resources"></a>為何要使用電腦的本機資源？
您可能想要使用電腦的本機資源來：  
  
-   在沒有可連到虛擬機器的網路連線情況下，對虛擬機器進行疑難排解。  
  
-   以與使用「遠端桌面連線」(RDP) 相同的方式，將檔案複製及貼到虛擬機器或從該處複製及貼上。  
  
-   使用智慧卡來登入虛擬機器。  
  
-   從虛擬機器列印到本機印表機。  
  
-   在不使用 RDP 的情況下，對需要 USB 和健全重新導向的開發人員應用程式進行測試和疑難排解。  
  
## <a name="see-also"></a>另請參閱  
[連接到虛擬機器](https://technet.microsoft.com/library/cc742407.aspx)  
[應該在 HYPER-V 中建立 1 或 2 代虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)



