---
title: 建立新的 NIC 團隊主機電腦上 VM
description: 本主題提供 NIC 的聯合設定的相關資訊，讓您了解您必須，讓您在 Windows Server 2016 中設定新 NIC 團隊時選取的項目。
manager: brianlic
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
ms.openlocfilehash: 6a9866d1f4e72b3c77c3233b5e5582d250cfe6a2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>建立新的 NIC 團隊主機電腦上 VM

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題提供 NIC 的聯合設定的相關資訊，讓您了解您必須，讓您設定新 NIC 團隊時選取的項目。 本主題包含下列各節。  
  
-   [選擇小組模式](#bkmk_teaming)  
  
-   [選擇負載平衡模式](#bkmk_lb)  
  
-   [選擇待命介面卡設定](#bkmk_standby)  
  
-   [使用主要小組介面屬性](#bkmk_primary)  
  
> [!NOTE]  
> 如果您已經了解這些設定項目，您可以使用下列程序，設定 NIC 小組。  
>   
> -   [建立新 NIC 團隊，在 VM 中](../../technologies/nic-teaming/Create-a-New-NIC-Team-in-a-VM.md)  
> -   [建立新的 NIC 小組](../../technologies/nic-teaming/Create-a-New-NIC-Team.md)  
  
當您建立新的 NIC 團隊時，您必須設定下列 NIC 小組屬性。  
  
-   小組名稱  
  
-   會員卡  
  
-   小組模式  
  
-   負載平衡模式  
  
-   待命介面卡  
  
您也可以選擇設定主要小組介面並設定 virtual 區域網路 (VLAN) 數字。  
  
下圖範例屬性的值某些 NIC 小組所在顯示這些 NIC 小組屬性。  
  
![NIC 小組屬性](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  
  
## <a name="bkmk_teaming"></a>選擇小組模式  
這些選項 Teaming 模式**切換獨立**，**靜態小組**，並**連結彙總控制通訊協定 (LACP)**。 靜態小組與 LACP 是切換相關模式。 為獲得最佳的所有三種 Teaming 模式 NIC 小組效能，建議您使用的動態通訊負載平衡模式。  
  
**切換不受影響**  
  
切換獨立模式、切換或 NIC 小組成員連接都不知道的卡 NIC 小組的成員，並不判斷如何 NIC 小組成員-網路流量的參數反而 NIC 小組分散輸入的網路流量 NIC 小組的成員。  
  
當您的動態通訊使用切換獨立模式時，根據修改動態負載平衡演算法所 TCP 連接埠地址湊散發網路流量載入。 動態負載平衡演算法重新分配，以便個人流程傳輸可以從一個作用中的小組成員移到另一個最佳化小組成員頻寬利用流程。 演算法考量，轉散發流量都可能造成-單傳遞封包，因此所需的步驟來降到最低的可能性太可能。  
  
**切換相關**  
  
切換相關模式，若要切換的 NIC 小組成員連接判斷如何輸入的網路流量的 NIC 小組成員。 切換已完成獨立如何網路流量分配 NIC 小組的成員。  
  
> [!IMPORTANT]  
> 切換相關小組需要的所有小組成員都連接到相同的實體切換或多底座切換的共用多底座之間切換來電顯示。  
  
靜態 Teaming 需要您手動設定開關切換至和主機找出團隊的連結。 因為這是靜態設定的方案，還有協助開關切換至任何其他通訊協定，並找出正確主機插入纜長度或其他錯誤都可能造成失敗執行小組。 伺服器級參數通常被支援此模式。  
  
然而靜態團隊合作，LACP 小組模式動態辨識連接主機和開關切換至之間的連結。 此動態連接可自動建立的團隊，理論上但很少做法、擴充和降低信用團隊的只要傳輸或收到從等實體 LACP 封包。 所有伺服器級參數都支援 LACP，而所有的網路電信業者以系統管理員讓 LACP 切換連接埠。 當您設定的 LACP Teaming 模式時，NIC 小組永遠運作 LACP 的作用中模式中的簡短計時器。  不是目前可用來修改計時器或變更 LACP 模式。  
  
當您的動態通訊使用切換相關模式時，根據修改動態負載平衡演算法所 TransportPorts 位址湊散發網路流量載入。  動態負載平衡演算法重新分配最佳化小組成員頻寬利用流程。 個人流程傳輸可移動使用團隊成員到另一個動態通訊的一部分。 演算法考量，轉散發流量都可能造成-單傳遞封包，因此所需的步驟來降到最低的可能性太可能。  
  
在所有切換相關設定、使用切換判斷如何輸入流量的小組成員。  切換預期地合理流量分散至所有的小組成員，但會有以判斷它會如何的完全獨立。  
  
## <a name="bkmk_lb"></a>選擇負載平衡模式  
這些選項負載平衡 distribution 模式**位址 Hash**，**HYPER-V 連接埠**，並**動態**。  
  
**地址 Hash**  
  
此負載平衡模式下建立 hash 為基礎的地址元件封包。 然後，指派的其中一個可用的介面卡該 hash 值封包。 通常只這個機制是不足，無法使用的介面卡上建立合理的餘額。  
  
您可以使用 Windows PowerShell 來指定下列 hashing 函式元件值。  
  
-   來源和目的地的 TCP 連接埠和來源和目的地的 IP 位址。 此為預設值，當您選取 [**位址 Hash**以負載平衡模式。  
  
-   來源和目的地的 IP 位址只。  
  
-   來源和目的地的 MAC 位址只。  
  
TCP 連接埠湊建立資料傳輸串流，導致小串流可以獨立 NIC 小組成員間移動，最細微的散發。 不過，您無法使用 TCP 連接埠湊的流量的 TCP 不是或 UDP 為基礎，或位置的 TCP 與 UDP 連接埠隱藏從堆疊，例如 IPsec 保護資料傳輸。 在這些案例中，會自動湊使用的 IP 位址湊或 MAC 位址湊如果流量不 IP 流量，就會使用。  
  
**HYPER-V 連接埠**  
  
使用 HYPER-V 連接埠模式 NIC 團隊 HYPER-V 主機上的設定是優點。 Vm 有獨立的 MAC 地址，因為 VM 的 MAC 位址-或 VM HYPER-V Virtual 開關已連接到連接埠-可以來進行除法運算 NIC 小組成員間網路流量的基礎。  
  
> [!IMPORTANT]  
> 使用 HYPER-V 連接埠負載平衡模式 NIC 團隊，在 Vm 中建立無法設定。 使用位址湊改為負載平衡模式。  
  
相鄰切換永遠看見一個連接埠上有特定的 MAC 地址，因為開關切換至會在多個連結根據目的地 MAC (VM MAC) 位址分配輸入載入（流量來自切換至主機）。 這是適合一樣佇列 (VMQs) 使用時，因為特定而流量希望抵達有佇列。  
  
不過，如果主機只有少數 Vm，此模式下可能無法達到細微達成平衡良好的通訊。 此模式下也將會限制單一 VM（亦即，從單一切換連接埠流量）可在單一介面頻寬。 NIC 小組而不使用的來源的 MAC 地址，因為可能會以多個連接埠切換的 MAC 位址設定 VM 有時候，id 使用 HYPER-V 虛擬切換連接埠。  
  
**動態**  
  
此負載平衡模式使用最佳的兩種模式中的每個層面，並將它們結合成單一的模式：  
  
-   輸出載入分散根據 hash 的 TCP 連接埠和 IP 位址。  動態模式也 rebalances 載入即時使輸出指定的流程可能前和往後小組成員之間移動。  
  
-   輸入的載入視訊光碟相同的方式做為 HYPER-V 連接埠模式。  
  
在此模式下輸出載入的動態平衡依據 flowlets 的概念。 人性化語音有端的文字與句子自然休息，如同 TCP 流程（TCP 通訊串流）也有自然發生符號。 兩個這類分符號間流量的 TCP 的部分稱為 flowlet。  
  
動態模式演算法偵測到的 flowlet 邊界遇到-例如時 TCP 流程-中發生的不足長度分演算法必要時，會自動 rebalances 給其他小組成員流程。  在有時演算法可能也會定期重新平衡不包含任何 flowlets 流程。 因此，相關性之間 TCP 流程和小組成員可以變更在任何時候動態平衡演算法平衡的小組成員工作負載的運作方式。  
  
小組設定切換獨立或切換相關模式的其中之一，建議您使用動態 distribution 模式來獲得最佳效能。  
  
還有此規則例外時 NIC 小組有兩個小組成員，設定切換獨立模式，有一個 NIC 作用中的功能，主動日待命模式和其他待命的設定。 透過此 NIC 小組設定，位址 Hash distribution 提供稍微提升效能比動態 distribution。  
  
## <a name="bkmk_standby"></a>選擇待命介面卡設定  
待命介面卡有無（所有的網路卡主動式）或 NIC 團隊，做為待命介面卡未選取的小組時使用中的特定網路介面卡的選擇。 當您設定 NIC 待命介面卡時，網路流量是傳送到或處理的介面卡，除非，直到作用中的 NIC 失敗時。 作用中的 NIC 失敗之後，待命而變成作用中，並處理程序網路流量。 如果和所有小組成員會都還原到服務，待命小組成員會傳回至待命狀態。  
  
如果您有兩個-NIC 團隊，並選擇一個 NIC 設定為待命介面卡，您遺失的頻寬，有兩種使用中 Nic 彙總優點。  
  
> [!IMPORTANT]  
> 您不需要指定待命介面卡，以達成容錯。容錯都有時 NIC 小組中有兩個以上的網路介面卡。  
  
## <a name="bkmk_primary"></a>使用主要小組介面屬性  
若要存取主要小組介面對話方塊中，您必須按一下連結以下圖表反白顯示。  
  
![主要小組介面屬性](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
按下反白顯示的連結，以下後**新小組介面**對話方塊。  
  
![新的小組介面對話方塊](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
如果您使用 Vlan，您可以使用此對話方塊中，指定的 VLAN 數字。  
  
不論您使用 Vlan，您可以指定 tNIC NIC 小組的名稱。  
  
## <a name="see-also"></a>也了  
[建立新 NIC 團隊，在 VM 中](../../technologies/nic-teaming/Create-a-New-NIC-Team-in-a-VM.md)  
[NIC 小組](NIC-Teaming.md)  
  


