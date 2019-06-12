---
title: 在主機電腦或 VM 上建立新的 NIC 小組
description: 本主題中，您會建立新的 NIC 小組，在主機電腦上或在執行 Windows Server 2016 的 HYPER-V 虛擬機器 (VM)。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: d4dc7e0795d1f1d0b2a8bc18a6df12c683ef037d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812342"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>在主機電腦或 VM 上建立新的 NIC 小組

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，您會建立新的 NIC 小組，在主機電腦上或在執行 Windows Server 2016 的 HYPER-V 虛擬機器 (VM)。  

## <a name="network-configuration-requirements"></a>網路設定需求  
您可以建立新的 NIC 小組之前，您必須部署兩個連接到不同的實體交換器的網路介面卡的 HYPER-V 主機。 您也必須設定網路介面卡來自相同的 IP 位址範圍的 IP 位址。  

實體交換器、 HYPER-V 虛擬交換器、 區域網路 (LAN)、 與 VM 中建立一個 NIC 小組的 NIC 小組需求如下：  

-   執行 HYPER-V 的電腦必須有兩個或多個網路介面卡。  

-   如果連接到多部實體交換器的網路介面卡，實體交換器必須位於相同的第 2 層子網路。  

-   您必須使用 HYPER-V 管理員 或 Windows PowerShell 來建立兩個外部的 HYPER-V 虛擬交換器，每個連接至不同的實體網路介面卡。  

-   VM 必須連接到您建立這兩個外部虛擬交換器。  

-   NIC 小組，在 Windows Server 2016 中，支援使用在 Vm 中的兩個成員的小組。 您可以建立較大規模的小組，但不支援。

-   如果您要在 VM 中設定 NIC 小組，您必須選取**小組模式**的_交換器獨立_並**負載平衡模式**的_位址雜湊_. 

## <a name="step-1-configure-the-physical-and-virtual-network"></a>步驟 1. 設定實體和虛擬網路  
在此程序中，您可以建立兩個外部的 HYPER-V 虛擬交換器、 將 VM 連接到交換器，然後設定交換器的 VM 連線。  

### <a name="prerequisites"></a>必要條件

您必須擁有成員資格**系統管理員**，或同等權限。  

### <a name="procedure"></a>程序

1.  在 HYPER-V 主機上，開啟 [HYPER-V 管理員] 中，並按一下 [動作] 底下**虛擬交換器管理員**。  

   ![虛擬交換器管理員](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  

2.  在 [虛擬交換器管理員]，請確定**外部**已選取，然後按一下**建立虛擬交換器**。  

   ![建立虛擬交換器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  

3.  在 虛擬交換器內容，輸入**名稱**的虛擬交換器，並新增**備忘稿**視。  

4.  在 **連線類型**，請在**外部網路**，選取您要連接的虛擬交換器的實體網路介面卡。  

5.  設定您的部署，額外的參數屬性，然後按一下**確定**。  

6.  重複上述步驟，以建立第二個的外部虛擬交換器。 第二個的外部交換器連線至不同的網路介面卡。  

7.  在 HYPER-V 管理員 中，在**虛擬機器**，以滑鼠右鍵按一下您想要設定，然後按一下 VM**設定**。  

   VM**設定**對話方塊隨即開啟。

8.  請確定不會啟動 VM。 如果啟動時，在關閉之前執行的 VM 設定。  

8.  在 **硬體**，按一下**網路介面卡**。  

   ![網路介面卡](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  

9. 在 **網路介面卡**屬性，選取您在先前步驟中建立的虛擬交換器的其中一個，然後按一下**套用**。  

    ![選取的虛擬交換器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  

10. 在 **硬體**，按一下以展開旁邊的加號 （+）**網路介面卡**。 

11. 按一下 **進階功能**啟用 NIC 小組使用圖形化使用者介面。 

    >[!TIP]
    >您也可以啟用 NIC 小組使用 Windows PowerShell 命令： 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```

    a. 選取 **動態**MAC 位址。 

    b. 按一下以選取**受保護網路**。 

    c. 按一下以選取**啟用此網路介面卡，以在客體作業系統中之小組的**。 

    d. 按一下 [確定]  。  

    ![將網路介面卡新增至小組](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  

13. 在中新增第二個網路介面卡，在 HYPER-V 管理員 中，**虛擬機器**相同的 VM，以滑鼠右鍵按一下，然後按一下 **設定**。  

    VM**設定**對話方塊隨即開啟。

14. 在 **新增硬體**，按一下**網路介面卡**，然後按一下**新增**。  

   ![新增網路介面卡](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  

15. 在 **網路介面卡**屬性，選取您在先前步驟中建立第二個虛擬交換器，然後按一下**套用**。  

   ![適用於虛擬交換器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  

16. 在 **硬體**，按一下以展開旁邊的加號 （+）**網路介面卡**。 

17. 按一下 **進階功能**，向下捲動至**NIC 小組**，然後按一下以選取**啟用此網路介面卡，以在客體作業系統中之小組的**。 

18. 按一下 [確定]  。  

_**恭喜您 ！** _  您已設定的實體和虛擬網路。  現在您可以繼續建立新的 NIC 小組。  

## <a name="step-2-create-a-new-nic-team"></a>步驟 2. 建立新的 NIC 小組

當您建立新的 NIC 小組時，您必須設定的 NIC 小組 」 屬性：  

-   小組名稱  

-   成員介面卡  

-   小組模式  

-   負載平衡模式  

-   待命的配接器  

您也可以設定主要小組介面，並設定虛擬 LAN (VLAN) 數字。  

如需這些設定的詳細資訊，請參閱 < [NIC 小組設定](nic-teaming-settings.md)。

### <a name="prerequisites"></a>先決條件

您必須擁有成員資格**系統管理員**，或同等權限。  

### <a name="procedure"></a>程序

1. 在 [伺服器管理員] 中，按一下 [本機伺服器]  。  

2. 在**屬性**窗格中，在第一欄中，尋找**NIC 小組**，然後按一下**已停用**連結。  

   **NIC 小組**對話方塊隨即開啟。

   ![[NIC 小組] 對話方塊](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)

3. 在 **配接器和介面**，選取您想要新增至 NIC 小組的一或多個網路介面卡。  

4. 按一下 **工作**，然後按一下**將加入至新的小組**。  

   **新的小組** 對話方塊隨即開啟並顯示網路介面卡和小組成員。

5. 在 **小組名稱**，輸入新的 NIC 小組的名稱，然後按一下**其他屬性**。  

6. 在 **其他屬性**，針對選取的值：

   - **小組模式**。 小組模式的選項都會**交換器獨立**並**交換器相依**。 交換器相依模式包含**靜態小組**並**連結彙總控制通訊協定 (LACP)** 。 

     - **交換器獨立。** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

     - **交換器相依。** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 


       |                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
       |----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
       |              **靜態小組**              |                                                                                                                                              您需要手動設定參數和主應用程式，以找出哪些連結形成小組。 因為這是以靜態方式設定的解決方案時，沒有任何其他的通訊協定，可協助交換器和主機，以識別不正確插入纜線或其他錯誤，可能會造成小組失敗來執行。 伺服器等級的交換器通常會支援這個模式。                                                                                                                                              |
       | **連結彙總控制通訊協定 (LACP)** | 不同於靜態小組 LACP 小組模式動態識別於主機與交換器之間連線的連結。 這個動態的連線可以自動建立的小組，理論上，但很少作法、 擴充和縮減小組中，只要傳輸或接收 LACP 封包，從對等實體。 所有伺服器等級的交換器都支援 LACP，和所有需要網路操作員，由系統管理員啟用 LACP 交換器連接埠上。 當您設定的 LACP 小組模式時，NIC 小組一律運作 LACP 的主動模式中使用簡短的計時器。  若要修改的計時器，或變更 LACP 模式目前使用任何選項不。 |

       ---

   - **負載平衡模式**。 負載平衡分配模式的選項都會**位址雜湊**， **HYPER-V 通訊埠**，並**動態**。

     - **位址雜湊。** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

     - **HYPER-V 通訊埠。** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

     - **動態的。** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

   - **待命的配接器**。 待命配接器的選項都會**無 （所有介面卡使用）** 或您選擇的 NIC 小組，做為待命配接器中的特定網路介面卡。

   > [!TIP]  
   > 如果您要設定 NIC 小組在虛擬機器 (VM) 中，您必須選取**小組模式**的_交換器獨立_並**負載平衡模式**的_位址雜湊_。  

7. 如果您想要設定主要小組介面名稱，或將 VLAN 號碼指派給 NIC 小組，按一下右邊的連結**主要小組介面**。  

    **新的小組介面**對話方塊隨即開啟。

    ![新的小組介面](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)

8. 根據您的需求，執行下列其中一項動作：  

   -   提供 tNIC 介面名稱。  

   -   設定 VLAN 成員資格： 按一下**特定 VLAN**輸入 VLAN 資訊。 例如，如果您想要將此 NIC 小組新增至 帳戶處理的 VLAN 編號 44，型別會計 44-VLAN。   

9. 按一下 [確定]  。  

_**恭喜您 ！** _  您已在主機電腦或 VM 上建立新的 NIC 小組。

## <a name="related-topics"></a>相關主題

- [NIC 小組](NIC-Teaming.md):在本主題中，我們提供您的網路介面卡 (NIC) 小組概觀 Windows Server 2016 中。 NIC 小組可讓您將介於 1 到 32 到一或多個以軟體為基礎的虛擬網路介面卡的實體 Ethernet 網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。   

- [NIC 小組 MAC 位址使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md):當您設定 NIC 小組與交換器獨立模式和位址雜湊或動態配置資源負載分配時，小組所使用的媒體存取控制 (MAC) 位址的輸出流量的主要 NIC 小組成員。 主要的 NIC 小組成員會將作業系統從一組初始的小組成員所選取的網路介面卡。

- [NIC 小組設定](nic-teaming-settings.md):本主題中，我們會提供您的 NIC 小組 」 屬性，例如小組的概觀，以及負載平衡模式。 我們也提供您關於待命配接器設定和主要小組介面屬性的詳細資料。 如果您有至少兩個網路介面卡在 NIC 小組時，您不需要指定容錯移轉的待命介面卡。

- [疑難排解 NIC 小組](Troubleshooting-NIC-Teaming.md):本主題中，我們會討論如何疑難排解 「 NIC 小組，例如硬體、 實體交換器 securities 和停用或啟用使用 Windows PowerShell 的網路介面卡。 

---