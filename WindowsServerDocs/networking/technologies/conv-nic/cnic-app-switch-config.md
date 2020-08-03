---
title: 聚合式 NIC 的實體交換器設定
description: 在本主題中，我們會提供您設定實體交換器的指導方針。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/14/2018
ms.openlocfilehash: 8d227098fb23b233b416cb9342a15d6d4ca0699e
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87520217"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>聚合式 NIC 的實體交換器設定

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，我們會提供您設定實體交換器的指導方針。

這些只是命令及其用途;您必須判斷在您的環境中連接 Nic 的通訊埠。

>[!IMPORTANT]
>請確定已針對設定 SMB 的優先順序設定 VLAN 和 no drop 原則。

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Arista switch \( dc \- 7050s \- 64，EOS \- 4.13.7 m\)

1.  en \( 移至系統管理員模式，通常會要求輸入密碼\)
2.  \(要進入設定模式的 config\)
3.  顯示回合 \( 顯示目前正在執行的設定\)
4.  找出您的 Nic 所連線的交換器埠。 在這些範例中，它們是 14/1、15/1、16/1、17/1。
5.  int eth 14/1，15/1，16/1，17/1 \( 進入設定模式，適用于這些埠\)
6.  dcbx 模式 ieee
7.  優先順序-流量控制模式開啟
8.  交換器匯流排原生 vlan 225
9.  交換器匯流排允許 vlan 100-225
10. 模式下的主幹
11. 優先順序-流程式控制制優先順序3不放
12. qos 信任 cos
13. 顯示 [執行] \( 確認設定已正確地安裝在埠上\)
14. wr \( ，讓設定在交換器重新開機時持續保存\)

### <a name="tips"></a>祕訣：
1.  否 #command # 對命令進行否定
2.  如何新增 VLAN： \( 如果儲存體網路位於 vlan 100，則為 int vlan 100\)
3.  如何檢查現有的 Vlan：顯示 vlan
4.  如需設定 Arista 交換器的詳細資訊，請在線上搜尋： Arista EOS Manual
5.  使用此命令來確認 PFC 設定：顯示優先順序-流程式控制制計數器詳細資料

## <a name="dell-switch-s4810-ftos-99-00"></a>Dell 交換器 \( S4810，FTOS 9.9 \( 0。0\)\)

```
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
```

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Cisco switch \( 結點3132，version 6.0 \( 2 \) U6 \( 1\)\)

### <a name="global"></a>全球

```
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
```

### <a name="port-specific"></a>特定埠

```
    switchport mode trunk
    switchport trunk native vlan 99
    switchport trunk allowed vlan 99,2000,2050   çuse VLANs that already exists
    spanning-tree port type edge
    flowcontrol receive on (not supported with PFC in Cisco NX-OS)
    flowcontrol send on (not supported with PFC in Cisco NX-OS)
    no shutdown
    priority-flow-control mode on
```

## <a name="related-topics"></a>相關主題

- [具有單一網路介面卡的聚合式 NIC 設定](cnic-single.md)
- [聚合式 NIC 組合 NIC 設定](cnic-datacenter.md)
- [針對聚合式 NIC 設定進行疑難排解](cnic-app-troubleshoot.md)
