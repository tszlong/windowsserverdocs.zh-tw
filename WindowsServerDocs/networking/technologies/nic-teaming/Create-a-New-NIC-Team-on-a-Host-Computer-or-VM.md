---
title: 在主機電腦或 VM 上建立新的 NIC 小組
description: 在本主題中，您會在主機電腦或執行 Windows Server 2016 的 Hyper-v 虛擬機器（VM）中建立新的 NIC 小組。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 1785b34741ce525a5bdd27b77a0e52fc2ca6c1b6
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591109"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>在主機電腦或 VM 上建立新的 NIC 小組

>適用於：Windows Server (半年通道)、Windows Server 2016

在本主題中，您會在主機電腦或執行 Windows Server 2016 的 Hyper-v 虛擬機器（VM）中建立新的 NIC 小組。  

## <a name="network-configuration-requirements"></a>網路設定需求  
建立新的 NIC 小組之前，您必須部署具有兩個網路介面卡的 Hyper-v 主機，以連線至不同的實體交換器。 您也必須使用來自相同 IP 位址範圍的 IP 位址來設定網路介面卡。  

在 VM 中建立 NIC 小組的實體交換器、Hyper-v 虛擬交換器、區域網路（LAN）和 NIC 小組需求如下：  

-   執行 Hyper-v 的電腦必須有兩個以上的網路介面卡。  

-   如果將網路介面卡連接到多個實體交換器，實體交換器必須位於相同的第2層子網。  

-   您必須使用 Hyper-v 管理員或 Windows PowerShell 來建立兩個外部 Hyper-v 虛擬交換器，每個都連接到不同的實體網路介面卡。  

-   VM 必須連接到您所建立的兩個外部虛擬交換器。  

-   Windows Server 2016 中的 NIC 小組支援在 Vm 中有兩個成員的團隊。 您可以建立更大的小組，但不支援。

-   如果您要在 VM 中設定 NIC 小組，您必須選取 [_交換器獨立_] 和 [_位址雜湊_的**負載平衡模式]** 的**分組模式**。 

## <a name="step-1-configure-the-physical-and-virtual-network"></a>步驟 1. 設定實體和虛擬網路  
在此程式中，您會建立兩個外部 Hyper-v 虛擬交換器、將 VM 連線至交換器，然後設定與交換器的 VM 連線。  

### <a name="prerequisites"></a>必要條件

您必須具有**Administrators**的成員資格或同等許可權。  

### <a name="procedure"></a>程序

1.  在 Hyper-v 主機上，開啟 [Hyper-v 管理員]，然後在 [動作] 底下，按一下 [**虛擬交換器管理員**]。  

   ![虛擬交換器管理員](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  

2.  在 [虛擬交換器管理員] 中，確認已選取 [**外部**]，然後按一下 [**建立虛擬交換器**]。  

   ![建立虛擬交換器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  

3.  在 [虛擬交換器內容] 中，輸入虛擬交換器的**名稱**，並視需要新增**附注**。  

4.  在 [連線**類型**] 的 [**外部網路**] 中，選取您要連接虛擬交換器的實體網路介面卡。  

5.  為您的部署設定其他切換屬性，然後按一下 **[確定]** 。  

6.  重複先前的步驟，以建立第二個外部虛擬交換器。 將第二個外部交換器連接到不同的網路介面卡。  

7.  在 [Hyper-v 管理員] 的 [**虛擬機器**] 底下，以滑鼠右鍵按一下您要設定的 VM，然後按一下 [**設定**]。  

   [VM**設定**] 對話方塊隨即開啟。

8.  確定 VM 未啟動。 如果已啟動，請在設定 VM 之前先執行關閉。  

8.  在 [**硬體**] 中，按一下 [**網路介面卡**]。  

   ![網路介面卡](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  

9. 在 [**網路介面卡**內容] 中，選取您在先前步驟中建立的其中一個虛擬交換器，**然後按一下 [** 套用]。  

    ![選取虛擬交換器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  

10. 在 [**硬體**] 中，按一下以展開 [**網路介面卡**] 旁的加號（+）。 

11. 按一下 [ **Advanced Features** ]，使用圖形化使用者介面啟用 NIC 小組。 

    >[!TIP]
    >您也可以使用 Windows PowerShell 命令來啟用 NIC 小組： 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```

    a. 選取 [**動態**] 做為 MAC 位址。 

    b。 按一下以選取 [**受保護的網路**]。 

    c. 按一下以選取 **[啟用此網路介面卡成為客體作業系統中的小組的**成員]。 

    d. 按一下 **\[確定\]** 。  

    ![將網路介面卡新增至小組](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  

13. 若要新增第二個網路介面卡，請在 Hyper-v 管理員 的 **虛擬機器**中，以滑鼠右鍵按一下相同的 VM，然後按一下 **設定**。  

    [VM**設定**] 對話方塊隨即開啟。

14. 在 [**新增硬體**] 中，按一下 [**網路介面卡**]，然後按一下 [**新增**]。  

   ![新增網路介面卡](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  

15. 在 [**網路介面卡**內容] 中，選取您在先前步驟中建立的第二個虛擬交換器，**然後按一下 [** 套用]。  

   ![套用虛擬交換器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  

16. 在 [**硬體**] 中，按一下以展開 [**網路介面卡**] 旁的加號（+）。 

17. 按一下 [ **Advanced Features**]，向下流覽至 [ **NIC**小組]，然後按一下以選取 **[啟用此網路介面卡成為客體作業系統中的小組的**成員]。 

18. 按一下 **\[確定\]** 。  

_**那麼!**_  您已設定實體和虛擬網路。  現在您可以繼續建立新的 NIC 小組。  

## <a name="step-2-create-a-new-nic-team"></a>步驟 2. 建立新的 NIC 小組

當您建立新的 NIC 小組時，必須設定 NIC 小組屬性：  

-   小組名稱  

-   成員介面卡  

-   小組模式  

-   負載平衡模式  

-   待命介面卡  

您也可以選擇性地設定主要小組介面和設定虛擬 LAN （VLAN）編號。  

如需這些設定的詳細資訊，請參閱[NIC 小組設定](nic-teaming-settings.md)。

### <a name="prerequisites"></a>必要條件

您必須具有**Administrators**的成員資格或同等許可權。  

### <a name="procedure"></a>程序

1. 在 [伺服器管理員] 中，按一下 [本機伺服器]。  

2. 在 [**屬性**] 窗格的第一個資料行中，找出 [ **NIC**小組]，然後按一下 [**停用**] 連結。  

   [ **NIC**小組] 對話方塊隨即開啟。

   ![[NIC 小組] 對話方塊](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)

3. 在 [**介面卡和介面**] 中，選取您要新增至 NIC 小組的一或多個網路介面卡。  

4. 按一下 [工作]，**然後按一下 [** **加入至新小組**]。  

   [**新增小組**] 對話方塊隨即開啟，並顯示網路介面卡和小組成員。

5. 在 [**小組名稱**] 中，輸入新 NIC 小組的名稱，然後按一下 [**其他屬性**]。  

6. 在 [**其他屬性**] 中，選取 [值]：

   - 小組**模式**。 小組模式的選項與交換器**無關**，而且**切換相依**。 交換器相依模式包含**靜態**小組和**連結匯總控制通訊協定（LACP）** 。 

     - **獨立切換。** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

     - **切換相依。** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 


       |                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
       |----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
       |              **靜態小組**              |                                                                                                                                              需要您手動設定交換器和主機，以識別哪些連結形成小組。 因為這是靜態設定的解決方案，所以沒有額外的通訊協定可協助交換器和主機找出不正確的插入纜線或其他可能導致小組無法執行的錯誤。 伺服器等級的交換器通常會支援這個模式。                                                                                                                                              |
       | **連結匯總控制通訊協定（LACP）** | 與靜態小組不同的是，LACP 小組模式會以動態方式識別主機與交換器之間連線的連結。 這個動態連線能夠自動建立小組，理論上，但在實務上，您只需透過傳輸或接收來自對等實體的 LACP 封包，就能擴充和減少小組。 所有伺服器類別參數都支援 LACP，而全部都需要網路操作員在交換器埠上以系統管理的方式啟用 LACP。 當您設定 LACP 的小組模式時，NIC 小組一律會以 LACP 的主動模式運作。  根據預設，NIC 小組會使用短暫的計時器（3秒），但您可以使用 `Set-NetLbfoTeam` 設定長計時器（90秒）。 |

       ---

   - **負載平衡模式**。 負載平衡分配模式的選項包括**位址雜湊**、 **Hyper-v 埠**和**動態**。

     - **位址雜湊。** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

     - **Hyper-v 通訊埠。** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

     - **效果.** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

   - **待命介面卡**。 待命介面卡的選項為 [**無] （所有可用的介面卡）** ，或您在 NIC 小組中選取作為待命介面卡的特定網路介面卡。

   > [!TIP]  
   > 如果您要在虛擬機器（VM）中設定 NIC 小組，您必須選取 [_交換器獨立_] 和 [_位址雜湊_的**負載平衡模式]** 這**組模式**。  

7. 如果您想要設定主要小組介面名稱，或將 VLAN 號碼指派給 NIC 小組，請按一下 [**主要小組介面**] 右邊的連結。  

    [**新增小組介面**] 對話方塊隨即開啟。

    ![新增小組介面](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)

8. 視您的需求而定，執行下列其中一項：  

   -   提供 tNIC 介面名稱。  

   -   設定 VLAN 成員資格：按一下 [**特定 VLAN** ]，然後輸入 vlan 資訊。 例如，如果您想要將此 NIC 小組新增至會計 VLAN 編號44，請輸入 Accounting 44-VLAN。   

9. 按一下 **\[確定\]** 。  

_**那麼!**_  您已在主機電腦或 VM 上建立新的 NIC 小組。

## <a name="related-topics"></a>相關主題

- [NIC](NIC-Teaming.md)小組：在本主題中，我們將概述 Windows Server 2016 中的網路介面卡（NIC）小組。 NIC 小組可讓您將一和32實體 Ethernet 網路介面卡分成一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。   

- [NIC 小組 MAC 位址使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md)：當您使用「交換器獨立模式」和「位址雜湊」或「動態」負載分佈來設定 NIC 小組時，小組會在輸出上使用主要 NIC 小組成員的媒體存取控制（MAC）位址流量. 主要 NIC 小組成員是由一組初始小組成員的作業系統選取的網路介面卡。

- [Nic 小組設定](nic-teaming-settings.md)：在本主題中，我們將概述 nic 團隊的屬性，例如團隊和負載平衡模式。 我們也會提供有關待命介面卡設定和主要小組介面屬性的詳細資料。 如果您的 NIC 小組中至少有兩張網路介面卡，則不需要指定待命介面卡來容錯。

- 針對[nic 小組進行疑難排解](Troubleshooting-NIC-Teaming.md)：在本主題中，我們會討論針對 nic 小組進行疑難排解的方法，例如硬體、實體交換器的安全性，以及使用 Windows PowerShell 來停用或啟用網路介面卡。 

---
