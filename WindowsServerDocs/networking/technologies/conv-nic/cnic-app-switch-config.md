---
title: 聚合型 NIC 實體切換設定
description: 本主題是匯集 NIC 設定節目表適用於 Windows Server 2016 的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 98a2e249aea38bd4d07dc1bcbc9b1ca98b98b6d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="physical-switch-configuration-for-converged-nic"></a>聚合型 NIC 實體切換設定

>適用於：Windows Server（以每年次管道）、Windows Server 2016

設定您的實體參數，您可以為指導方針使用下列的各節。

這些是只命令與他們的使用。您必須在您的環境中判斷 Nic 的連接埠。 

>[!IMPORTANT]
>請確定的 VLAN 和無拖原則設定的 SMB 所設定的優先順序。

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>切換 arista \ (dcs\-7050s\-64，EOS\-4.13.7M\)

1.  短破折號 \（請移至系統管理員模式，password\ 通常會詢問）
2.  設定 \（將輸入設定 mode\）
3.  顯示執行 \]（顯示目前執行 configuration\）
4.  了解您 Nic 連接到的切換連接埠。 在這些範例中，其為 14 日 1,15 日 1,16 日 1,17 日 1。
5.  Int 式乙太網路 14 日 1,15 日 1,16 日 1,17 月 1 \（輸入這些 ports\ 組態模式）
6.  Dcbx 模式 ieee
7.  優先順序流量控制模式
8.  Switchport 主幹原生 vlan 225
9.  允許 vlan 100-225 switchport 主幹
10. Switchport 模式主幹
11. 無放優先順序流量控制優先順序 3
12. Qos 信任 cos
13. 顯示執行 \（確認該設定正確 ports\ 上）
14. Wr \（若要讓 [設定] 跨持續存在切換 reboot\）

### <a name="tips"></a>秘訣：
1.  不 #command # 否定命令
2.  如何新增新的 VLAN: int vlan 100 \（如果儲存網路上的 VLAN 100\）
3.  如何查看現有 Vlan：顯示 vlan
4.  如需有關設定 Arista 切換，搜尋 online: Arista EOS 手動
5.  使用這個命令來確認 PFC 設定：顯示優先順序流量控制計數器詳細資料

## <a name="dell-switch-s4810-ftos-99-00"></a>Dell 切換 \ (S4810，FTOS 9.9 \(0.0\)\)

    
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
    

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Cisco 切換 \ (關係 3132、版本 6.0\(2\)U6\(1\)\)

### <a name="global"></a>全球
    
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
    

## <a name="all-topics-in-this-guide"></a>本指南所有主題

本指南包含下列主題。

- [使用單一網路介面卡的聚合型的 NIC 設定](cnic-single.md)
- [聚合型的 NIC 小組的 NIC 設定](cnic-datacenter.md)
- [聚合型 NIC 實體切換設定](cnic-app-switch-config.md)
- [疑難排解匯集 NIC 設定](cnic-app-troubleshoot.md)
