---
title: 在主機電腦或 VM 上建立新的 NIC 小組
description: 在本主題中，您會在主機電腦或 Hyper-v 虛擬機器中建立新的 NIC 小組， (VM) 執行 Windows Server 2016。
manager: dougkim
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.topic: article
ms.openlocfilehash: b7a75a610695348d7a42a2fea92fd24bb565966e
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946854"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>在主機電腦或 VM 上建立新的 NIC 小組

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，您會在主機電腦或 Hyper-v 虛擬機器中建立新的 NIC 小組， (VM) 執行 Windows Server 2016。

## <a name="network-configuration-requirements"></a>網路設定需求
建立新的 NIC 小組之前，您必須先部署有兩張網路介面卡連接到不同實體交換器的 Hyper-v 主機。 您也必須使用來自相同 IP 位址範圍的 IP 位址來設定網路介面卡。

在 VM 中建立 NIC 小組的實體交換器、Hyper-v 虛擬交換器、區域網路 (LAN) 和 NIC 小組需求為：

-   執行 Hyper-v 的電腦必須有兩個以上的網路介面卡。

-   如果將網路介面卡連線到多個實體交換器，實體交換器必須位於相同的第2層子網。

-   您必須使用 Hyper-v 管理員或 Windows PowerShell 來建立兩個外部 Hyper-v 虛擬交換器，每個都連接到不同的實體網路介面卡。

-   VM 必須連接到您建立的兩個外部虛擬交換器。

-   Windows Server 2016 中的 NIC 小組支援在 Vm 中具有兩個成員的團隊。 您可以建立較大的小組，但不支援。

-   如果您要在 VM 中設定 NIC 小組，您必須選取 _獨立交換器_ 的小組 **模式**，以及 _位址雜湊_ 的 **負載平衡模式**。

## <a name="step-1-configure-the-physical-and-virtual-network"></a>步驟 1： 設定實體和虛擬網路
在此程式中，您會建立兩個外部 Hyper-v 虛擬交換器、將 VM 連線到交換器，然後設定對交換器的 VM 連接。

### <a name="prerequisites"></a>先決條件

您必須具有 **Administrators** 的成員資格或同等許可權。

### <a name="procedure"></a>程序

1.  在 Hyper-v 主機上，開啟 [Hyper-v 管理員]，然後在 [動作] 底下，按一下 [ **虛擬交換器管理員**]。

   ![虛擬交換器管理員](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)

2.  在 [虛擬交換器管理員] 中，確認已選取 [ **外部** ]，然後按一下 [ **建立虛擬交換器**]。

   ![建立虛擬交換器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)

3.  在 [虛擬交換器內容] 中，輸入虛擬交換器的 **名稱** ，並視需要新增 **附注** 。

4.  在 [連線 **類型**] 的 [ **外部網路**] 中，選取您要連接虛擬交換器的實體網路介面卡。

5.  為您的部署設定其他切換屬性，然後按一下 **[確定]**。

6.  重複上述步驟來建立第二個外部虛擬交換器。 將第二個外部交換器連接到不同的網路介面卡。

7.  在 [Hyper-v 管理員] 的 [ **虛擬機器**] 下，以滑鼠右鍵按一下您要設定的 VM，然後按一下 [ **設定**]。

   [VM **設定** ] 對話方塊隨即開啟。

8.  確定未啟動 VM。 如果已啟動，請先執行關機，再設定 VM。

8.  在 [ **硬體**] 中，按一下 [ **網路介面卡**]。

   ![網路介面卡](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)

9. 在 [ **網路介面卡** 內容] 中，選取您在先前步驟中建立的其中一個虛擬交換器， **然後按一下 [** 套用]。

    ![選取虛擬交換器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)

10. 在 [ **硬體**] 中，按一下以展開 [ **網路介面卡**] 旁的加號 (+) 。

11. 按一下 [ **Advanced Features** ]，使用圖形化使用者介面啟用 NIC 小組。

    >[!TIP]
    >您也可以使用 Windows PowerShell 命令啟用 NIC 小組：
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```

    a. 選取 [ **動態** MAC 位址]。

    b. 按一下以選取 [ **受保護的網路**]。

    c. 按一下以選取 **[啟用此網路介面卡成為客體作業系統中的小組的一部分**]。

    d. 按一下 [確定]  。

    ![將網路介面卡新增至小組](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)

13. 若要新增第二張網路介面卡，請在 [Hyper-v 管理員] 的 [ **虛擬機器**] 中，以滑鼠右鍵按一下相同的 VM，然後按一下 [ **設定**]。

    [VM **設定** ] 對話方塊隨即開啟。

14. 在 [ **新增硬體**] 中，按一下 [ **網路介面卡**]，然後按一下 [ **新增**]。

   ![新增網路介面卡](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)

15. 在 [ **網路介面卡** 內容] 中，選取您在先前步驟中建立的第二個虛擬交換器， **然後按一下 [** 套用]。

   ![套用虛擬交換器](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)

16. 在 [ **硬體**] 中，按一下以展開 [ **網路介面卡**] 旁的加號 (+) 。

17. 按一下 [ **Advanced Features**]、向下滾動至 [ **NIC** 小組]，然後按一下 **[啟用此網路介面卡成為客體作業系統中的小組成員**]。

18. 按一下 [確定]。

_**恭喜！**_  您已設定實體和虛擬網路。  現在您可以繼續建立新的 NIC 小組。

## <a name="step-2-create-a-new-nic-team"></a>步驟 2： 建立新的 NIC 小組

當您建立新的 NIC 小組時，您必須設定 NIC 小組屬性：

-   球隊名稱

-   成員介面卡

-   小組模式

-   負載平衡模式

-   待命介面卡

您也可以選擇性地設定主要小組介面，並設定虛擬 LAN (VLAN) 號碼。

如需這些設定的詳細資訊，請參閱 [NIC 小組設定](nic-teaming-settings.md)。

### <a name="prerequisites"></a>先決條件

您必須具有 **Administrators** 的成員資格或同等許可權。

### <a name="procedure"></a>程序

1. 在 [伺服器管理員] 中，按一下 [本機伺服器]。

2. 在 [ **屬性** ] 窗格的第一個資料行中，找出 [ **NIC** 小組]，然後按一下 [ **停用** ] 連結。

   [ **NIC** 小組] 對話方塊隨即開啟。

   ![[NIC 小組] 對話方塊](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)

3. 在 [ **介面卡和介面**] 中，選取您要新增至 NIC 小組的一或多個網路介面卡。

4. 按一下 [工作 **]，然後按一下 [****加入至新的團隊**]。

   [ **新增小組** ] 對話方塊隨即開啟，並顯示網路介面卡和團隊成員。

5. 在 [ **小組名稱**] 中，輸入新 NIC 小組的名稱，然後按一下 [ **其他屬性**]。

6. 在 [ **其他屬性**] 中，選取下列值：

   - 小組 **模式**。 小組模式的選項是 **獨立切換** 和 **切換相依** 的選項。 交換器相依模式包括 **靜態** 小組和 **連結匯總控制通訊協定 (LACP)**。

     - **獨立切換。** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

     - **切換相依。** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]


       |                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
       |----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
       |              **靜態團隊**              |                                                                                                                                              需要您手動設定交換器和主機，以識別哪些連結形成小組。 因為這是靜態設定的解決方案，所以沒有額外的通訊協定可協助交換器和主機識別不正確的插入纜線或其他可能導致小組無法執行的錯誤。 伺服器等級的交換器通常會支援這個模式。                                                                                                                                              |
       | **連結匯總控制通訊協定 (LACP)** | 不同于靜態小組，LACP 小組模式會動態識別在主機與交換器之間連線的連結。 這項動態連接可讓您自動建立小組，但在理論上，只要透過從對等實體傳輸或接收 LACP 封包，就能擴展和縮減團隊。 所有伺服器等級的參數都支援 LACP，而且全部都需要網路操作員以系統管理員的身份啟用交換器埠上的 LACP。 當您設定 LACP 的小組模式時，NIC 小組一律會以 LACP 的主動模式運作。  根據預設，NIC 小組會使用短暫的計時器 (3 秒) ，但您可以 (90 秒) 設定較長的計時器 `Set-NetLbfoTeam` 。 |

       ---

   - **負載平衡模式**。 負載平衡分配模式的選項包括 **位址雜湊**、 **Hyper-v 埠** 和 **動態**。

     - **位址雜湊。** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

     - **Hyper-v 埠。** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

     - **動態。** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

   - **待命介面卡**。 待命介面卡的選項為 [無] (所有使用中的 **介面卡)** 或您在 NIC 小組中選取作為待命介面卡的特定網路介面卡。

   > [!TIP]
   > 如果您要在虛擬機器中設定 NIC 小組 (VM) ，您必須選取 _獨立交換器_ 的小組 **模式**，以及 _位址雜湊_ 的 **負載平衡模式**。

7. 如果您想要設定主要小組介面名稱或指派 VLAN 號碼給 NIC 小組，請按一下 [ **主要小組介面**] 右邊的連結。

    [ **新增小組介面** ] 對話方塊隨即開啟。

    ![新增小組介面](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)

8. 視您的需求而定，執行下列其中一項：

   -   提供 tNIC 介面名稱。

   -   設定 VLAN 成員資格：按一下 [ **特定 vlan** ]，然後輸入 vlan 資訊。 例如，如果您想要將此 NIC 小組新增到帳戶處理 VLAN 號碼44，請輸入 Accounting 44-VLAN。

9. 按一下 [確定]。

_**恭喜！**_  您已在主機電腦或 VM 上建立新的 NIC 小組。

## <a name="related-topics"></a>相關主題

- [NIC](NIC-Teaming.md)小組：在本主題中，我們將概述 Windows Server 2016 中 (NIC) 小組的網路介面卡。 NIC 小組可讓您將32一或多個實體 Ethernet 網路介面卡分組成一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。

- [NIC 小組 MAC 位址使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md)：當您設定具有交換器獨立模式的 NIC 小組，以及位址雜湊或動態負載散發時，小組會使用輸出流量上主要 NIC 小組成員的媒體存取控制 (MAC) 位址。 主要 NIC 小組成員是由作業系統從一組初始的團隊成員所選取的網路介面卡。

- [Nic 小組設定](nic-teaming-settings.md)：在此主題中，我們將概述 NIC 小組屬性，例如小組和負載平衡模式。 此外，我們也會提供待命介面卡設定和主要小組介面屬性的詳細資料。 如果 NIC 小組中至少有兩張網路介面卡，您就不需要指定待命介面卡來容錯。

- [疑難排解 nic](Troubleshooting-NIC-Teaming.md)小組：在本主題中，我們將討論如何針對 nic 小組進行疑難排解，例如硬體、實體交換器的安全性，以及使用 Windows PowerShell 來停用或啟用網路介面卡。

---
