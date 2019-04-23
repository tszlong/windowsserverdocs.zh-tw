---
title: 交集的 NIC 的實體交換器組態
description: 本主題中，我們提供您的指導方針來設定您的實體交換器。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: e31d7b83fee84d9055d938f77b49389205786244
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829399"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>交集的 NIC 的實體交換器組態

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，我們提供您的指導方針來設定您的實體交換器。 


這些是只有命令和其用法;在您的環境中，您必須決定為 Nic 連線的連接埠。 

>[!IMPORTANT]
>確定 VLAN 和不置放原則已設定為針對 SMB 所設定的優先順序。

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Arista 交換器\(網域控制站\-7050s年\-64，EOS\-4.13.7M\)

1.  en\(移至 系統管理模式，通常會要求密碼\)
2.  設定\(進入設定模式\)
3.  顯示執行\(顯示目前執行中的設定\)
4.  了解將您的 Nic 所連接到的交換器連接埠。 在這些範例中，它們是 14/1,15/1,16/1,17/1。
5.  int eth 14/1,15/1,16/1,17/1\(進入設定模式，這些連接埠\)
6.  dcbx 模式 ieee
7.  優先順序流量控制模式
8.  switchport 主幹原生 vlan 225
9.  switchport 主幹允許 vlan 100 225
10. switchport 模式主幹
11. 優先順序流量控制優先順序 3 不置放
12. qos 信任 cos
13. 顯示執行\(確認該設定是正確的連接埠上的安裝程式\)
14. 寫入\(以使設定持續存在於所有交換器重新開機\)

### <a name="tips"></a>秘訣：
1.  沒有 #command # 變換正負號的命令
2.  如何新增新的 VLAN: int vlan 100\(如果儲存體網路上的 VLAN 100\)
3.  如何檢查現有的虛擬區域網路： vlan 的示範
4.  如需有關設定 Arista 參數，線上搜尋的詳細資訊：Arista EOS 手動
5.  若要確認 PFC 設定使用此命令： 顯示優先順序流量控制計數器詳細資料

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Dell 交換器\(S4810、 FTOS 9.9 \(0.0\)\)

    
    !
    dcb enable
    ! put pfc control on qos class 3
    configure
    dcb-map dcb-smb
    priority group 0 bandwidth 90 pfc on
    priority group 1 bandwidth 10 pfc off
    priority-pgid 1 1 1 0 1 1 1 1
    exit
    ! apply map to ports 0-31
    configure
    interface range ten 0/0-31
    dcb-map dcb-smb
    exit
    
--- 

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Cisco 交換器\(Nexus 3132，版本 6.0\(2\)U6\(1\)\)

### <a name="global"></a>全域
    
    class-map type qos match-all RDMA
    match cos 3
    class-map type queuing RDMA
    match qos-group 3
    policy-map type qos QOS_MARKING
    class RDMA
    set qos-group 3
    class class-default
    policy-map type queuing QOS_QUEUEING
    class type queuing RDMA
    bandwidth percent 50
    class type queuing class-default
    bandwidth percent 50
    class-map type network-qos RDMA
    match qos-group 3
    policy-map type network-qos QOS_NETWORK
    class type network-qos RDMA
    mtu 2240
    pause no-drop
    class type network-qos class-default
    mtu 9216
    system qos
    service-policy type qos input QOS_MARKING
    service-policy type queuing output QOS_QUEUEING
    service-policy type network-qos QOS_NETWORK
    

### <a name="port-specific"></a>特定的連接埠

    
    switchport mode trunk
    switchport trunk native vlan 99
    switchport trunk allowed vlan 99,2000,2050   çuse VLANs that already exists
    spanning-tree port type edge
    flowcontrol receive on (not supported with PFC in Cisco NX-OS)
    flowcontrol send on (not supported with PFC in Cisco NX-OS)
    no shutdown
    priority-flow-control mode on
    
--- 

## <a name="related-topics"></a>相關主題

- [具有單一網路介面卡的交集的 NIC 設定](cnic-single.md)
- [交集的 NIC 組合的 NIC 設定](cnic-datacenter.md)
- [疑難排解交集 NIC 組態](cnic-app-troubleshoot.md)

--- 