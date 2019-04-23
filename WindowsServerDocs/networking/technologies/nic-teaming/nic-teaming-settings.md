---
title: NIC 小組設定
description: 本主題中，我們會提供您的 NIC 小組 」 屬性，例如小組的概觀，以及負載平衡模式。 我們也提供您關於待命配接器設定和主要小組介面屬性的詳細資料。 如果您有至少兩個網路介面卡在 NIC 小組時，您不需要指定容錯移轉的待命介面卡。
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
ms.openlocfilehash: 57957e88ff4c398be23355534d5cc0ad7f920bb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877929"
---
# <a name="nic-teaming-settings"></a>NIC 小組設定
本主題中，我們會提供您的 NIC 小組 」 屬性，例如小組的概觀，以及負載平衡模式。 我們也提供您關於待命配接器設定和主要小組介面屬性的詳細資料。 如果您有至少兩個網路介面卡在 NIC 小組時，您不需要指定容錯移轉的待命介面卡。


  
![NIC 組合屬性](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  

## <a name="teaming-modes"></a>小組模式 
小組模式的選項都會**交換器獨立**並**交換器相依**。 交換器相依模式包含**靜態小組**並**連結彙總控制通訊協定 (LACP)**。 

>[!TIP]
>為了達到最佳的 NIC 小組效能，我們建議您使用動態通訊群組的負載平衡模式。  
  
### <a name="switch-independent"></a>交換器獨立
  
[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)] 
  
當您將交換器獨立模式使用動態通訊時，網路流量負載被分散修改動態負載平衡演算法為基礎的 TCP 通訊埠位址雜湊。 動態負載平衡演算法會轉散發流程來最佳化小組成員的頻寬使用率，以便個別的流量傳輸可以有一個作用中的小組成員之間移動。 此演算法會考慮到極小的可能，轉散發流量可能會造成的順序傳遞的封包，因此接受該可能性降至最低的步驟。  
  
### <a name="switch-dependent"></a>交換器相依  

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]  
  
> [!IMPORTANT]  
> 所有小組成員都連線至相同的實體交換器，或多重底座切換的交換器相依小組需要共用的多個底座切換識別碼。


- **靜態小組。** 靜態小組需要您手動設定參數和主應用程式，以找出哪些連結形成小組。 因為這是以靜態方式設定的解決方案時，沒有任何其他的通訊協定，可協助交換器和主機，以識別不正確插入纜線或其他錯誤，可能會造成小組失敗來執行。 伺服器等級的交換器通常會支援這個模式。

- **連結彙總控制通訊協定 (LACP)。** 不同於靜態小組 LACP 小組模式動態識別於主機與交換器之間連線的連結。 這個動態的連線可以自動建立的小組，理論上，但很少作法、 擴充和縮減小組中，只要傳輸或接收 LACP 封包，從對等實體。 所有伺服器等級的交換器都支援 LACP，和所有需要網路操作員，由系統管理員啟用 LACP 交換器連接埠上。 當您設定的 LACP 小組模式時，NIC 小組一律運作 LACP 的主動模式中使用簡短的計時器。  若要修改的計時器，或變更 LACP 模式目前使用任何選項不。


當您將交換器相依模式使用動態通訊時，網路流量負載被分散根據 TransportPorts 位址雜湊，以修改動態負載平衡演算法。  動態負載平衡演算法會轉散發流程，以最佳化小組成員的頻寬使用率。 個別的流量傳輸可以從一個作用中的小組成員間移動動態通訊群組的一部分。 此演算法會考慮到極小的可能，轉散發流量可能會造成的順序傳遞的封包，因此接受該可能性降至最低的步驟。  
  
因為所有的交換器相依組態中，交換器會決定如何散發小組成員之間的流量。  參數應該是合理的工作，將流量分散到小組成員但它有完整的獨立性，以判斷它的運作方式。  


## <a name="load-balancing-modes"></a>負載平衡模式  
負載平衡分配模式的選項都會**位址雜湊**， **HYPER-V 通訊埠**，並**動態**。  
  
### <a name="address-hash"></a>位址雜湊
  
[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
  
使用 Windows PowerShell 來指定下列的雜湊函式元件的值。  
  
-   來源和目的地 TCP 連接埠及來源和目的地 IP 位址。 這是預設值，當您選取**位址雜湊**做為負載平衡模式。  
  
-   來源和目的地 IP 位址只。  
  
-   來源和目的地 MAC 位址僅。  
  
TCP 連接埠雜湊會建立最細微的流量資料流，因而產生較小的資料流可以 NIC 小組成員之間獨立移動分佈。 不過，您無法使用 TCP 連接埠雜湊流量不是 TCP 或 UDP 型，或其中的 TCP 和 UDP 連接埠會從堆疊隱藏，例如受 IPsec 保護流量。 在這些情況下，雜湊會自動使用 IP 位址雜湊，或如果該流量不是 IP 流量，會使用 MAC 位址雜湊。  
  
### <a name="hyper-v-port"></a>HYPER-V 通訊埠
  
[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]  
  
相鄰的交換器一律會看到一個連接埠上的特定 MAC 位址，因為參數輸入負責將負載分散 （的流量從交換器到主機） 上的目的地 MAC (VM MAC) 位址為基礎的多個連結。 這是特別有用的虛擬機器佇列 (Vmq) 使用時，因為佇列可以放在特定位置的流量應該抵達的 NIC。  
  
不過，如果主機有只有少數的 Vm，此模式可能不細微，以達到平衡良好的分佈。 此模式也一律會限制單一 VM （亦即，從單一的交換器連接埠的流量） 是在單一介面上的 可用的頻寬。 NIC 小組可做為 HYPER-V 虛擬交換器連接埠的識別碼，而不是使用的來源 MAC 位址，因為在某些情況下，VM 可能設定多個交換器連接埠上的 MAC 位址。  
  
### <a name="dynamic"></a>動態
  
[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
  
在此模式中的輸出負載動態地平衡根據 flowlets 的概念。 就像人類的語音有字詞和句子結尾處的自然中斷，TCP 流量 （TCP 通訊資料流） 也會有自然發生中斷。 兩個這類符號之間 TCP 流量的部分被指 flowlet。  
  
動態模式演算法來偵測，flowlet 界限發現-例如何時長度足夠的中斷發生在 TCP 流程-演算法會在必要時，自動重新平衡流向另一個小組成員。  在某些情況下，演算法可能也會定期重新平衡不包含任何 flowlets 的流程。 因為這個緣故，TCP 流程和小組成員之間的親和性可以變更在任何時候，動態載入平衡演算法處理以平衡小組成員的工作負載。  
  
小組使用交換器獨立或其中一種交換器相依模式設定，建議您為了達到最佳效能，使用動態通訊模式。  
  
NIC 小組有兩個小組成員、 設定在交換器獨立模式中，並已啟用，以及一個使用中的 NIC 的作用中/待命模式，而另一個設定為待命，沒有此規則的例外狀況。 與此 NIC 小組設定中，位址雜湊散發會提供稍微較佳的效能比動態通訊群組。  


## <a name="standby-adapter-setting"></a>待命的配接器設定  
待命配接器的選項都會**無 （所有介面卡使用）** 或您選擇的 NIC 小組，做為待命配接器中的特定網路介面卡。 當您設定 NIC 為待命配接器時，所有其他的未選取的小組成員都是作用中，並沒有網路流量傳送至或配接器所處理，直到作用中的 NIC 失敗。 作用中的 NIC 失敗之後，變成作用中待命 NIC，並處理網路流量。 當所有小組成員取得都還原到服務時，待命的小組成員就會回到待命狀態。  

如果您有兩個 NIC 小組，而且您選擇為待命介面卡設定一個 NIC，您遺失頻寬彙總的優點在於具有兩個使用中的 Nic。  您不需要指定的待命配接器，以達成容錯功能;容錯移轉一律會出現在 NIC 小組中有至少兩個網路介面卡。
 
  
## <a name="primary-team-interface-property"></a>主要小組介面屬性  
若要存取 [主要小組介面] 對話方塊中，您必須按一下會在下圖中反白顯示的連結。  
  
![主要小組介面屬性](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
按一下 反白顯示的連結，也就是下列之後**新的小組介面**對話方塊隨即開啟。  
  
![[新增小組介面] 對話方塊](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
如果您要使用 Vlan，您可以使用此對話方塊來指定 VLAN 號碼。  
  
不論是否使用的 Vlan，您可以指定 NIC 小組的 tNIC 名稱。  
  


---