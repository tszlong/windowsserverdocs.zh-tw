---
title: 針對融合式 NIC 設定進行疑難排解
description: 本主題是 Windows Server 2016 的交集 NIC 配置指南的一部分。
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a4d62be747859c84247b4afaa9e5a9276c072ea0
ms.sourcegitcommit: 8e330f9066097451cd40e840d5f5c3317cbc16c2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2020
ms.locfileid: "97697020"
---
# <a name="troubleshooting-converged-nic-configurations"></a>針對融合式 NIC 設定進行疑難排解

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用下列腳本來確認 Hyper-v 主機上的 RDMA 設定是否正確。

- [下載腳本 Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

您也可以使用下列 Windows PowerShell 命令來疑難排解和驗證交集 Nic 的設定。

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

若要確認您的網路介面卡 RDMA 設定，請在 Hyper-v 伺服器上執行下列 Windows PowerShell 命令。

```powershell
Get-NetAdapterRdma | fl *
```

在 Hyper-v 主機上執行此命令之後，您可以使用下列預期和非預期的結果來識別和解決問題。

### <a name="get-netadapterrdma-expected-results"></a>Get-NetAdapterRdma 預期的結果

主機 vNIC 和實體 NIC 顯示非零的 RDMA 功能。

![Windows PowerShell 預期的結果](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma 非預期的結果

如果您在執行 NetAdapterRdma 命令時收到 **非** 預期的結果，請執行下列步驟。

1. 請確定 Mlnx 微型埠和 Mlnx 匯流排驅動程式都是最新的。 若為 Mellanox，請至少使用 drop 42。
2. 透過裝置管理員檢查驅動程式版本，確認 Mlnx 微型埠和匯流排驅動程式相符。 匯流排驅動程式可在系統裝置中找到。 名稱的開頭應該是 Mellanox Connect-X 3 PRO VPI，如下列網路介面卡屬性的螢幕擷取畫面所示。

![網路介面卡內容](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. 請確定已在實體 NIC 和主機 vNIC 上啟用網路 Direct (RDMA) 。
5. 藉由檢查其 RDMA 功能，確定已透過正確的實體介面卡建立 vSwitch。
6. 檢查 EventViewer 系統記錄，並依來源「Hyper-v-VmSwitch」進行篩選。

## <a name="get-smbclientnetworkinterface-verifies-rdma-configuration"></a>Get-SmbClientNetworkInterface 驗證 RDMA 設定

另一個驗證 RDMA 設定的步驟是在 Hyper-v 伺服器上執行下列 Windows PowerShell 命令。

```powershell
Get-SmbClientNetworkInterface
```

### <a name="get-smbclientnetworkinterface-expected-results"></a>Get-SmbClientNetworkInterface 預期的結果

主機 vNIC 應該也會顯示為可從 SMB 的觀點來看的 RDMA。

![網路介面卡內容](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface 非預期的結果

1. 請確定 Mlnx 微型埠和 Mlnx 匯流排驅動程式都是最新的。 若為 Mellanox，請至少使用 drop 42。
2. 透過裝置管理員檢查驅動程式版本，確認 Mlnx 微型埠和匯流排驅動程式相符。 匯流排驅動程式可在系統裝置中找到。 名稱的開頭應該是 Mellanox Connect-X 3 PRO VPI，如下列網路介面卡屬性的螢幕擷取畫面所示。
3. 請確定已在實體 NIC 和主機 vNIC 上啟用網路 Direct (RDMA) 。
4. 藉由檢查其 RDMA 功能，確定已透過正確的實體介面卡建立 Hyper-v 虛擬交換器。
5. 檢查應用程式和服務中「SMB 用戶端」的 EventViewer 記錄 **|Microsoft |Windows**。

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

您可以執行 \( 下列 Windows PowerShell 命令，以查看網路介面卡的服務 QoS 設定品質 \) 。

```powershell
Get-NetAdapterQos
```

### <a name="get-netadapterqos-expected-results"></a>Get-NetAdapterQos 預期的結果

優先順序和流量類別應該根據您使用本指南執行的第一個設定步驟來顯示。

![服務優先順序和類別的品質](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-NetAdapterQos 非預期的結果

如果您的結果不是預期的，請執行下列步驟。

1. 確定實體網路介面卡支援資料中心橋接 \( DCB \) 和 QoS
2. 確定網路介面卡驅動程式是最新的。

## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

您可以使用下列 Windows PowerShell 命令來確認遠端節點的 IP 位址具有 RDMA \- 功能。

```powershell
Get-SmbMultiChannelConnection
```

### <a name="get-smbmultichannelconnection-expected-results"></a>Get-SmbMultiChannelConnection 預期的結果

遠端節點的 IP 位址會顯示為支援 RDMA。

![支援 RDMA 的遠端節點 IP 位址](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get-SmbMultiChannelConnection 非預期的結果

如果您的結果不是預期的，請執行下列步驟。

1. 請確定 ping 的運作方式。
2. 請確定防火牆未封鎖 SMB 連線起始。 具體而言，請啟用適用于 iWARP 的 SMB 直接埠5445的防火牆規則，以及 ROCE 的445。

## <a name="get-smbclientnetworkinterface-verifies-nic-is-rmda-capable"></a>Get-SmbClientNetworkInterface 驗證 NIC 是否具備 RMDA 功能

您可以使用下列命令來確認您為 RDMA 啟用的虛擬 NIC 回報為具有 \- SMB 功能的 rdma。

```powershell
Get-SmbClientNetworkInterface
```

### <a name="get-smbclientnetworkinterface-expected-results"></a>Get-SmbClientNetworkInterface 預期的結果

針對 RDMA 啟用的虛擬 NIC 必須視為可由 SMB 支援的 RDMA。

![SMB 報告 Nic 支援 RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface 非預期的結果

如果您的結果不是預期的，請執行下列步驟。

1. 請確定 ping 的運作方式。
2. 請確定防火牆未封鎖 SMB 連線起始。

## <a name="vstat-mellanox-specific"></a>vstat \( Mellanox 特定\)

如果您使用的是 Mellanox 網路介面卡，則可以使用 **vstat** 命令，透過 \( hyper-v 節點上的交集乙太網路 ROCE 版本驗證 RDMA \) 。

### <a name="vstat-expected-results"></a>vstat 預期的結果

這兩個節點上的 RoCE 版本必須相同。 這也是確認兩個節點的固件版本是否最新的好方法。

![RoCE 版本檢查結果範例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat 非預期的結果

如果您的結果不是預期的，請執行下列步驟。

1. 使用 Set-MlnxDriverCoreSetting 設定正確的 RoCE 版本
2. 從 Mellanox 網站安裝最新的固件。

## <a name="perfmon-counters"></a>Perfmon 計數器

您可以在效能監視器中檢查計數器，以驗證您設定的 RDMA 活動。

![效能監視器結果範例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

## <a name="related-topics"></a>相關主題

- [具有單一網路介面卡的交集 NIC 設定](cnic-single.md)
- [交集 NIC 分組 NIC 設定](cnic-datacenter.md)
- [融合式 NIC 的實體交換器設定](cnic-app-switch-config.md)
