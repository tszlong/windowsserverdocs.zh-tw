---
title: 疑難排解匯集 NIC 設定
description: 本主題是匯集 NIC 設定節目表適用於 Windows Server 2016 的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 373ecd9b9fff62aaabd8caa176ff091ec98ad81c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-converged-nic-configurations"></a>疑難排解匯集 NIC 設定

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要確認是否 RDMA 設定是否正確 HYPER-V 主機上，您可以使用下列的指令碼。

- [下載指令碼 Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

您也可以使用下列的 Windows PowerShell 命令疑難排解，並確認您聚合型 Nic 的設定。

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

若要確認您的網路介面卡 RDMA 設定，請執行下列 Windows PowerShell 命令 HYPER-V 伺服器上。

    
    Get-NetAdapterRdma | fl *
    

找出並修正問題的相關 HYPER-V 主機上執行此命令之後，您可以使用下列如預期般和未預期的結果。

### <a name="get-netadapterrdma-expected-results"></a>Get-NetAdapterRdma 預期結果

主機但 vNIC 和實體而會顯示為零 RDMA 功能。

![Windows PowerShell 預期結果](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma 未預期的結果

如果您收到執行時的非預期的結果，請執行下列步驟**取得-NetAdapterRdma**命令。

1. 請確定 Mlnx 迷你連接埠和 Mlnx 匯流排驅動程式的最新項目。 適用於 Mellanox，使用至少卸除 42。 
2. 驗證檢查裝置管理員] 中透過驅動程式版本符合 Mlnx 迷你連接埠和匯流排驅動程式。 在 [系統裝置可以找到匯流排驅動程式。 下列螢幕擷取畫面的網路介面卡屬性中所示 Mellanox Connect-X 3 PRO VPI，應該會開始名稱。

![網路介面卡屬性](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. 請務必在同時實體網路介面卡、主機但 vNIC 尚未網路直接 (RDMA)。
5. 請務必檢查其 RDMA 功能 vSwitch 建立透過向實體介面卡。
6. 檢查系統事件檢視器以登入和來源」超-V-VmSwitch」來篩選。

## <a name="get-smbclientnetworkinterface"></a>取得 SmbClientNetworkInterface

做為額外的步驟來驗證您的設定 RDMA，執行下列 Windows PowerShell 命令 HYPER-V 伺服器上。


    Get SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>取得如預期般 SmbClientNetworkInterface 結果

主機但 vNIC 應該會顯示為可 RDMA SMB 的觀點也。

![網路介面卡屬性](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>取得 SmbClientNetworkInterface 未預期的結果

1. 請確定 Mlnx 迷你連接埠和 Mlnx 匯流排驅動程式的最新項目。 適用於 Mellanox，使用至少卸除 42。 
2. 驗證檢查裝置管理員] 中透過驅動程式版本符合 Mlnx 迷你連接埠和匯流排驅動程式。 在 [系統裝置可以找到匯流排驅動程式。 下列螢幕擷取畫面的網路介面卡屬性中所示 Mellanox Connect-X 3 PRO VPI，應該會開始名稱。
3. 請務必在同時實體網路介面卡、主機但 vNIC 尚未網路直接 (RDMA)。
4. 請務必透過向實體介面卡建立 HYPER-V Virtual 開關切換至檢查其 RDMA 功能。
5. 事件檢視器以登檢查的「SMB Client」**應用程式與服務 |Microsoft |Windows**。

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

您可以檢視服務 \(QoS\) 設定網路介面卡的品質，執行下列 Windows PowerShell 命令。

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Get-NetAdapterQos 預期結果

根據您執行使用此快速入門第一次設定步驟，應該會顯示優先順序和流量類別。

![品質服務優先順序和類別](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-NetAdapterQos 未預期的結果

如果您的結果是發生未預期，執行下列步驟。

1. 確保您的資料中心橋接 \(DCB\) 和 QoS 實體網路介面卡的支援
2. 確定是最新的網路介面卡驅動程式。


## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

您可以使用下列的 Windows PowerShell 命令來確認 RDMA\ 能力的遠端節點 IP 位址。

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Get-SmbMultiChannelConnection 預期結果

遠端節點 IP 位址會顯示為 RDMA 功能。

![RDMA 可遠端節點 IP 位址](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get-SmbMultiChannelConnection 未預期的結果

如果您的結果是發生未預期，執行下列步驟。

1. 請務必 ping 的兩種方式運作。
2. 請務必防火牆不會封鎖 SMB 連接起始。 具體而言，讓 SMB 直接連接埠的 iWARP 5445 防火牆規則及的 ROCE 445。

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

您可以使用下列命令，以確認您支援 RDMA virtual 而回報為 RDMA\ 處理能力，SMB。

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Get-SmbClientNetworkInterface 預期結果

Virtual NIC RDMA 已支援，必須視為 SMB，RDMA 功能。

![SMB 報告 Nic 可 RDMA 功能](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface 未預期的結果

如果您的結果是發生未預期，執行下列步驟。

1. 請務必 ping 的兩種方式運作。
2. 請務必防火牆不封鎖 SMB 連接起始。

## <a name="vstat-mellanox-specific"></a>Vstat \(Mellanox specific\)

如果您使用 Mellanox 網路介面卡，您可以使用**vstat**命令驗證 RDMA 透過 HYPER-V 節點上匯集乙太網路 \(RoCE\) 版本。

### <a name="vstat-expected-results"></a>如預期般 vstat 結果

這兩個節點上的 RoCE 版本必須是相同。 這也是驗證，這兩個節點上的韌體版本是最新的好方法。

![RoCE 版本核取結果範例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>Vstat 未預期的結果

如果您的結果是發生未預期，執行下列步驟。

1. 設定使用 Set-MlnxDriverCoreSetting 正確 RoCE 版本
2. 安裝最新的韌體從 Mellanox 網站。


## <a name="perfmon-counters"></a>效能計數器

您可以檢視計數器效能監視器，以確認您的設定的 RDMA 活動中。

![效能監視器結果範例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

## <a name="all-topics-in-this-guide"></a>本指南所有主題

本指南包含下列主題。

- [使用單一網路介面卡的聚合型的 NIC 設定](cnic-single.md)
- [聚合型的 NIC 小組的 NIC 設定](cnic-datacenter.md)
- [聚合型 NIC 實體切換設定](cnic-app-switch-config.md)
- [疑難排解匯集 NIC 設定](cnic-app-troubleshoot.md)
