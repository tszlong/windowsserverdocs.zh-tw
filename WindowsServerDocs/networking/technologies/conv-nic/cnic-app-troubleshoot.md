---
title: 針對聚合式 NIC 設定進行疑難排解
description: 本主題是 Windows Server 2016 的交集 NIC 設定指南的一部分。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 90021956ebe85ec2f4abfea0aaafd96daebfd82f
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87520207"
---
# <a name="troubleshooting-converged-nic-configurations"></a>針對聚合式 NIC 設定進行疑難排解

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用下列腳本來驗證 Hyper-v 主機上的 RDMA 設定是否正確。

- [下載腳本 Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

您也可以使用下列 Windows PowerShell 命令來進行疑難排解，並驗證交集式 Nic 的設定。

## <a name="get-netadapterrdma"></a>NetAdapterRdma

若要驗證您的網路介面卡 RDMA 設定，請在 Hyper-v 伺服器上執行下列 Windows PowerShell 命令。

```powershell
Get-NetAdapterRdma | fl *
```

在 Hyper-v 主機上執行此命令之後，您可以使用下列預期和非預期的結果來識別並解決問題。

### <a name="get-netadapterrdma-expected-results"></a>NetAdapterRdma 預期的結果

主機 vNIC 和實體 NIC 會顯示非零的 RDMA 功能。

![Windows PowerShell 預期的結果](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>NetAdapterRdma 非預期的結果

如果您在執行 NetAdapterRdma 命令時收到**非**預期的結果，請執行下列步驟。

1. 請確定 Mlnx 微型埠和 Mlnx 匯流排驅動程式是最新的。 若是 Mellanox，請至少使用 drop 42。
2. 透過 Device Manager 檢查驅動程式版本，確認 Mlnx 微型埠和匯流排驅動程式相符。 匯流排驅動程式可以在 [系統裝置] 中找到。 名稱應以 Mellanox Connect-X 3 PRO VPI 開頭，如網路介面卡屬性的下列螢幕擷取畫面所示。

![網路介面卡內容](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. 請確定實體 NIC 和主機 vNIC 上都已啟用網路直接（RDMA）。
5. 藉由檢查其 RDMA 功能，確定已透過正確的實體介面卡建立 vSwitch。
6. 檢查 EventViewer 系統記錄檔，並依來源「Hyper-v-VmSwitch」進行篩選。

## <a name="get-smbclientnetworkinterface"></a>SmbClientNetworkInterface

如需驗證 RDMA 設定的額外步驟，請在 Hyper-v 伺服器上執行下列 Windows PowerShell 命令。

```powershell
Get-SmbClientNetworkInterface
```

### <a name="get-smbclientnetworkinterface-expected-results"></a>SmbClientNetworkInterface 預期的結果

主機 vNIC 應該也會顯示為從 SMB 的觀點來看的 RDMA 功能。

![網路介面卡內容](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>SmbClientNetworkInterface 非預期的結果

1. 請確定 Mlnx 微型埠和 Mlnx 匯流排驅動程式是最新的。 若是 Mellanox，請至少使用 drop 42。
2. 透過 Device Manager 檢查驅動程式版本，確認 Mlnx 微型埠和匯流排驅動程式相符。 匯流排驅動程式可以在 [系統裝置] 中找到。 名稱應以 Mellanox Connect-X 3 PRO VPI 開頭，如網路介面卡屬性的下列螢幕擷取畫面所示。
3. 請確定實體 NIC 和主機 vNIC 上都已啟用網路直接（RDMA）。
4. 藉由檢查其 RDMA 功能，確定已透過正確的實體介面卡建立 Hyper-v 虛擬交換器。
5. 在 [**應用程式和服務] 中檢查「SMB 用戶端」的 EventViewer 記錄 |Microsoft |Windows**。

## <a name="get-netadapterqos"></a>Get-netadapterqos

您可以藉 \( \) 由執行下列 Windows PowerShell 命令，來查看服務 QoS 設定的網路介面卡品質。

```powershell
Get-NetAdapterQos
```

### <a name="get-netadapterqos-expected-results"></a>Get-netadapterqos 預期的結果

優先順序和流量類別應該根據您使用本指南執行的第一個設定步驟來顯示。

![服務優先順序和類別的品質](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-netadapterqos 非預期的結果

如果您的結果不是預期的，請執行下列步驟。

1. 確定實體網路介面卡支援資料中心橋接 \( DCB \) 和 QoS
2. 確定網路介面卡驅動程式是最新的。

## <a name="get-smbmultichannelconnection"></a>SmbMultiChannelConnection

您可以使用下列 Windows PowerShell 命令來確認遠端節點的 IP 位址是否 \- 具備 RDMA 功能。

```powershell
Get-SmbMultiChannelConnection
```

### <a name="get-smbmultichannelconnection-expected-results"></a>SmbMultiChannelConnection 預期的結果

遠端節點的 IP 位址會顯示為支援 RDMA。

![支援 RDMA 的遠端節點 IP 位址](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>SmbMultiChannelConnection 非預期的結果

如果您的結果不是預期的，請執行下列步驟。

1. 請確定 ping 適用于這兩種方式。
2. 請確定防火牆不會封鎖 SMB 連線起始。 具體而言，請啟用適用于 iWARP 的 SMB 直接埠5445和445的防火牆規則來進行 ROCE。

## <a name="get-smbclientnetworkinterface"></a>SmbClientNetworkInterface

您可以使用下列命令來確認您為 RDMA 啟用的虛擬 NIC 回報為 \- 受 SMB 支援的 rdma。

```powershell
Get-SmbClientNetworkInterface
```

### <a name="get-smbclientnetworkinterface-expected-results"></a>SmbClientNetworkInterface 預期的結果

啟用 RDMA 的虛擬 NIC 必須視為可由 SMB 支援的 RDMA。

![SMB 報告 Nic 具備 RDMA 功能](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>SmbClientNetworkInterface 非預期的結果

如果您的結果不是預期的，請執行下列步驟。

1. 請確定 ping 適用于這兩種方式。
2. 請確定防火牆未封鎖 SMB 連線初始化。

## <a name="vstat-mellanox-specific"></a>vstat \( Mellanox 特定\)

如果您使用的是 Mellanox 網路介面卡，您可以使用**vstat**命令來確認 hyper-v 節點上的 RDMA 是否超過聚合式 Ethernet \( RoCE \) 版本。

### <a name="vstat-expected-results"></a>vstat 預期的結果

這兩個節點上的 RoCE 版本必須相同。 這也是確認兩個節點上的固件版本是否最新的好方法。

![RoCE 版本檢查結果範例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat 非預期的結果

如果您的結果不是預期的，請執行下列步驟。

1. 使用 Set-MlnxDriverCoreSetting 設定正確的 RoCE 版本
2. 從 Mellanox 網站安裝最新的固件。

## <a name="perfmon-counters"></a>Perfmon 計數器

您可以在效能監視器中查看計數器，以確認您設定的 RDMA 活動。

![效能監視器結果範例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

## <a name="related-topics"></a>相關主題

- [具有單一網路介面卡的聚合式 NIC 設定](cnic-single.md)
- [聚合式 NIC 組合 NIC 設定](cnic-datacenter.md)
- [聚合式 NIC 的實體交換器設定](cnic-app-switch-config.md)
