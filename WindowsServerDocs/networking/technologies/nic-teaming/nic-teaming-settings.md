---
title: NIC 小組設定
description: 在本主題中，我們將概述 NIC 小組的屬性，例如團隊和負載平衡模式。 我們也會提供有關待命介面卡設定和主要小組介面屬性的詳細資料。 如果您的 NIC 小組中至少有兩張網路介面卡，則不需要指定待命介面卡來容錯。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-nict
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 133e44c83032976f08819529508b3990b6e78596
ms.sourcegitcommit: fdc3ce1992f4dd6ea1771479d525126abbbcfa72
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85256678"
---
# <a name="nic-teaming-settings"></a>NIC 小組設定
在本主題中，我們將概述 NIC 小組的屬性，例如團隊和負載平衡模式。 我們也會提供有關待命介面卡設定和主要小組介面屬性的詳細資料。 如果您的 NIC 小組中至少有兩張網路介面卡，則不需要指定待命介面卡來容錯。


  
![NIC 小組屬性](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  

## <a name="teaming-modes"></a>小組模式 
小組模式的選項與交換器**無關**，而且**切換相依**。 交換器相依模式包含**靜態**小組和**連結匯總控制通訊協定（LACP）**。 

>[!TIP]
>為了獲得最佳的 NIC 小組效能，我們建議您使用動態散發的負載平衡模式。  
  
### <a name="switch-independent"></a>交換器獨立
  
[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)] 
  
當您使用具有動態散發的交換器獨立模式時，系統會根據動態負載平衡演算法所修改的 TCP 埠位址雜湊來散發網路流量負載。 動態負載平衡演算法會重新散發流程，以優化小組成員頻寬使用量，讓個別流量傳輸可以從一個作用中的小組成員移到另一個。 此演算法會考慮轉散發流量可能會導致封包行程順序不好的情況，因此，它會採取步驟來將這種可能性降到最低。  
  
### <a name="switch-dependent"></a>切換相依  

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]  
  
> [!IMPORTANT]  
> 切換相依小組需要所有團隊成員都連接到相同的實體交換器或多底座交換器，以在多個底座之間共用交換器識別碼。


- **靜態小組。** 靜態小組要求您必須手動設定交換器和主機，以識別哪些連結形成小組。 因為這是靜態設定的解決方案，所以沒有額外的通訊協定可協助交換器和主機找出不正確的插入纜線或其他可能導致小組無法執行的錯誤。 伺服器等級的交換器通常會支援這個模式。

- **連結匯總控制通訊協定（LACP）。** 與靜態小組不同的是，LACP 小組模式會以動態方式識別主機與交換器之間連線的連結。 這個動態連線能夠自動建立小組，理論上，但在實務上，您只需透過傳輸或接收來自對等實體的 LACP 封包，就能擴充和減少小組。 所有伺服器類別參數都支援 LACP，而全部都需要網路操作員在交換器埠上以系統管理的方式啟用 LACP。 當您設定 LACP 的小組模式時，NIC 小組一律會以具有短暫計時器的 LACP 主動模式運作。  目前沒有任何選項可用來修改計時器或變更 LACP 模式。


當您使用具有動態散發的交換器相依模式時，系統會根據動態負載平衡演算法修改的 TransportPorts 位址雜湊來散發網路流量負載。  動態負載平衡演算法會重新散發流程，以優化小組成員頻寬使用率。 個別的流量傳輸可以從一個作用中的小組成員移至另一個，做為動態散發的一部分。 此演算法會考慮轉散發流量可能會導致封包行程順序不好的情況，因此，它會採取步驟來將這種可能性降到最低。  
  
如同所有的交換器相依設定，此參數會決定如何在小組成員之間散發輸入流量。  此參數預期會執行合理的作業，將流量分散到小組成員，但它完全獨立以判斷其運作方式。  


## <a name="load-balancing-modes"></a>負載平衡模式  
負載平衡分配模式的選項包括**位址雜湊**、 **Hyper-v 埠**和**動態**。  
  
### <a name="address-hash"></a>位址雜湊
  
[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
  
使用 Windows PowerShell 指定下列雜湊函陣列件的值。  
  
-   來源和目的地 TCP 埠，以及來源和目的地 IP 位址。 當您選取 [**位址雜湊**] 做為負載平衡模式時，這是預設值。  
  
-   僅限來源和目的地 IP 位址。  
  
-   僅限來源和目的地 MAC 位址。  
  
TCP 埠雜湊會建立最細微的流量串流散發，產生可在 NIC 小組成員之間獨立移動的較小串流。 不過，您不能將 TCP 埠雜湊用於不是 TCP 或 UDP 型的流量，或從堆疊隱藏 TCP 和 UDP 埠，例如與受 IPsec 保護的流量。 在這些情況下，雜湊會自動使用 IP 位址雜湊，如果流量不是 IP 流量，則會使用 MAC 位址雜湊。  
  
### <a name="hyper-v-port"></a>Hyper-V 通訊埠
  
[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]  
  
由於連續的交換器一律會在某個埠上看到特定的 MAC 位址，因此交換器會根據目的地 MAC （VM MAC）位址，將輸入負載（從交換器到主機的流量）散發到多個連結上。 這在使用虛擬機器佇列（Vmq 數量）時特別有用，因為佇列可以放在預期抵達流量的特定 NIC 上。  
  
不過，如果主機只有幾個 Vm，則此模式可能不夠細微，而無法達到良好平衡的散發。 此模式也一律會將單一 VM （也就是來自單一交換器埠的流量）限制為單一介面上可用的頻寬。 NIC 小組會使用 Hyper-v 虛擬交換器埠做為識別碼，而不是使用來源 MAC 位址，因為在某些情況下，VM 可能會在交換器埠上設定一個以上的 MAC 位址。  
  
### <a name="dynamic"></a>動態
  
[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
  
在此模式中的輸出負載，會根據 flowlets 的概念進行動態平衡。 正如人類語音在單字和句子的結尾處自然中斷，TCP 流量（TCP 通訊串流）也會發生自然的中斷。 兩個這類中斷之間的 TCP 流程部分稱為「flowlet」。  
  
當動態模式演算法偵測到已遇到的 flowlet 界限時（例如，當 TCP 流程中發生了足夠長度的中斷時），演算法會自動將流程重新平衡至另一個小組成員（如果適用的話）。  在某些情況下，演算法可能也會定期重新平衡不包含任何 flowlets 的流程。 因此，TCP 流程與小組成員之間的親和性可能會隨時變更，因為動態平衡演算法會運作，以平衡小組成員的工作負載。  
  
無論小組是以「獨立交換器」或其中一個「切換相依」模式來設定，建議您最好使用動態分配模式來達到最佳效能。  
  
當 NIC 小組只有兩個小組成員、以交換器獨立模式設定，並已啟用作用中/待命模式、有一個使用中的 NIC，另一個設定為待命時，此規則就會發生例外狀況。 透過此 NIC 小組設定，位址雜湊散發提供的效能會比動態散發更好。  


## <a name="standby-adapter-setting"></a>待命介面卡設定  
待命介面卡的選項為 [**無] （所有可用的介面卡）** ，或您在 NIC 小組中選取作為待命介面卡的特定網路介面卡。 當您將 NIC 設定為待命介面卡時，所有其他未選取的小組成員都是作用中，而且在作用中的 NIC 失敗之前，介面卡不會傳送或處理任何網路流量。 在作用中的 NIC 失敗之後，待命 NIC 會變成作用中，並處理網路流量。 當所有小組成員還原至服務時，待命小組成員會回到待命狀態。  

如果您有兩個 NIC 的小組，而且您選擇將一個 NIC 設定為待命介面卡，則會失去與兩個作用中 Nic 並存的頻寬匯總優點。  您不需要指定待命介面卡即可達到容錯功能;當 NIC 小組中至少有兩張網路介面卡時，容錯一律會出現。
 
  
## <a name="primary-team-interface-property"></a>主要小組介面屬性  
若要存取 [主要小組介面] 對話方塊，您必須按一下下圖中反白顯示的連結。  
  
![主要小組介面屬性](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
按一下反白顯示的連結之後，就會開啟下列 [**新的小組介面**] 對話方塊。  
  
![[新增小組介面] 對話方塊](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
如果您使用 Vlan，可以使用此對話方塊來指定 VLAN 號碼。  
  
無論您是否使用 Vlan，都可以為 NIC 小組指定 NIC 名稱。  
  


---
