---
title: 建立 HYPER-V 虛擬機器的虛擬交換器
description: 提供有關建立虛擬交換器，使用 HYPER-V 管理員或 Windows PowerShell 的指示
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fdc8063c-47ce-4448-b445-d7ff9894dc17
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: e7c43a1b9173d347a3b6d6e1f8bd9127c62bd081
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880219"
---
# <a name="create-a-virtual-switch-for-hyper-v-virtual-machines"></a>建立 HYPER-V 虛擬機器的虛擬交換器

>適用於：Windows 10，Windows Server 2016、 Microsoft HYPER-V Server 2016、windows Server 2019，Microsoft HYPER-V Server 2019
  
虛擬交換器可讓建立與其他電腦通訊的 HYPER-V 主機上的虛擬機器。 當您第一次在 Windows Server 上安裝 HYPER-V 角色時，您可以建立虛擬交換器。 若要建立其他虛擬交換器，請使用 HYPER-V 管理員] 或 [Windows PowerShell。 若要深入了解虛擬交換器，請參閱[HYPER-V 虛擬交換器](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
虛擬機器網路可以是一個複雜的主題。 而且有數種新的虛擬交換器功能，您可能想要使用 like [Switch Embedded Teaming (SET)](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md#bkmk_sswitchembedded)。 但基本的網路功能是很容易進行。 本主題涵蓋的內容，讓您可以在 HYPER-V 中建立網路的虛擬機器。 若要深入了解您可以設定您的網路基礎結構，檢閱[網路](../../../networking/Networking.md)文件。   
  
## <a name="BKMK_HyperVMan"></a>使用 HYPER-V 管理員建立虛擬交換器  
  
1.  開啟 [HYPER-V 管理員] 中，選取 HYPER-V 主機電腦的名稱。  
  
2.  選取 **動作** > **虛擬交換器管理員**。  
  
    ![螢幕擷取畫面顯示 [動作] 功能表選項 > 虛擬交換器管理員](../media/Hyper-V-Action-VSwitchManager.png)  
  
3.  選擇您想要的虛擬交換器的類型。  
  
    |連線類型|描述|  
    |-------------------|---------------|  
    |外部|可讓虛擬機器存取實體網路與伺服器和外部網路上的用戶端進行通訊。 在相同的 HYPER-V 伺服器之間的通訊方式，可讓虛擬機器。|  
    |內部|可讓相同的 HYPER-V 伺服器上的虛擬機器之間，以及虛擬機器和管理主機作業系統之間的通訊。|  
    |Private|只允許在相同的 HYPER-V 伺服器上的虛擬機器之間的通訊。 私人網路是與所有 HYPER-V 伺服器上的外部網路流量隔離。 這種類型的網路時，您必須建立隔離的網路環境，例如隔離的測試網域。|  
  
4.  選取 **建立虛擬交換器**。  
  
5.  新增虛擬交換器的名稱。  
  
6.  如果您選取 [外部]，選擇您想要使用的網路介面卡 (NIC) 以及下表中所述的任何其他選項。  
  
    ![顯示外部網路選項的螢幕擷取畫面](../media/Hyper-V-NewVSwitch-ExternalOptions.png)  
  
    |設定名稱|描述|  
    |----------------|---------------|  
    |允許管理作業系統共用此網路介面卡|如果您想要讓 HYPER-V 主機共用的虛擬交換器及 NIC 或 NIC 小組與虛擬機器，請選取此選項。 以此方式啟用，主應用程式可以使用任何您所設定的虛擬交換器服務品質 (QoS) 設定、 安全性設定或 HYPER-V 虛擬交換器的其他功能等。|  
    |啟用單一根目錄 I/O 虛擬化 (SR-IOV)|選取此選項，只有當您想要允許虛擬機器流量略過虛擬機器交換器，並直接移至實體 nic。 如需詳細資訊，請參閱 <<c0> [ 單一根目錄 I/O 虛擬化](https://technet.microsoft.com/library/dn641211.aspx#Sec4)海報隨附參考中：HYPER-V 網路功能。|  
  
7.  如果您想要隔離網路流量從管理 HYPER-V 主機作業系統或其他共用相同的虛擬交換器的虛擬機器，選取**啟用管理作業系統的虛擬 LAN 識別碼**。 您可以將任意數目的 VLAN ID，或保留預設值。 這是管理作業系統將用於所有的網路通訊，透過此虛擬交換器的虛擬 LAN 識別碼。  
  
    ![顯示的 VLAN ID 選項的螢幕擷取畫面](../media/Hyper-V-NewSwitch-VLAN.png)  
  
8.  按一下 [確定] 。  
  
9. 按一下 [ **是**]。  
  
    ![會顯示 「 暫止的變更可能會中斷網路連線 」 訊息的螢幕擷取畫面](../media/Hyper-V-NewVSwitch-DisruptNetwork.png)  
  
## <a name="BKMK_WPS"></a>使用 Windows PowerShell 建立虛擬交換器  
  
1.  在 Windows 桌面上，按一下 \[開始\] 按鈕，然後輸入 **Windows PowerShell** 名稱的任何一部分。  
  
2.  以滑鼠右鍵按一下 Windows PowerShell，然後選取**系統管理員身分執行**。  
  
3.  執行，以尋找現有的網路介面卡[Get-netadapter](https://technet.microsoft.com/library/jj130867.aspx) cmdlet。 記下您想要用於虛擬交換器的網路介面卡名稱。  
  
    ```  
    Get-NetAdapter  
    ```  
  
4.  使用建立虛擬交換器[New-vmswitch](https://technet.microsoft.com/library/hh848455.aspx) cmdlet。 例如，若要建立外部虛擬交換器名為 ExternalSwitch，使用乙太網路介面卡，且**允許管理作業系統共用此網路介面卡**開啟，執行下列命令。  
  
    ```  
    New-VMSwitch -name ExternalSwitch  -NetAdapterName Ethernet -AllowManagementOS $true  
    ```  
  
    若要建立內部交換器，請執行下列命令。  
  
    ```  
    New-VMSwitch -name InternalSwitch -SwitchType Internal  
    ```  
  
    若要建立私用的交換器，請執行下列命令。  
  
    ```  
    New-VMSwitch -name PrivateSwitch -SwitchType Private  
    ```  
  
更進階 Windows PowerShell 指令碼討論 Windows Server 2016 中的改進或新的虛擬交換器功能，請參閱 <<c0> [ 遠端直接記憶體存取和交換器內嵌小組](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。  

  
## <a name="next-step"></a>後續步驟  
[在 HYPER-V 中建立虛擬機器](Create-a-virtual-machine-in-Hyper-V.md)  
  


