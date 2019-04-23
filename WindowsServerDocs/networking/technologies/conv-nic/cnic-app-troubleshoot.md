---
title: 疑難排解交集 NIC 組態
description: 本主題是交集 NIC 組態指南 for Windows Server 2016 的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3004ff9d6fe874410c24d174755a6d26f99f8f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876479"
---
# <a name="troubleshooting-converged-nic-configurations"></a>疑難排解交集 NIC 組態

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用下列指令碼，以驗證是否已正確為 HYPER-V 主機上的 RDMA 設定。

- [下載指令碼測試 Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

您也可以使用下列 Windows PowerShell 命令進行疑難排解，並確認您交集的 Nic 的組態。

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

若要確認您的網路介面卡 RDMA 設定，請在 HYPER-V 伺服器上執行下列 Windows PowerShell 命令。

    
    Get-NetAdapterRdma | fl *
    

若要找出並解決問題，您在 HYPER-V 主機上執行此命令之後，您可以使用下列預期和非預期的結果。

### <a name="get-netadapterrdma-expected-results"></a>取得 NetAdapterRdma 預期結果

主機 vNIC 和實體 NIC 會示範非零 RDMA 功能。

![Windows PowerShell 預期結果](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma unexpected results

如果您在執行時，收到未預期的結果，請執行下列步驟**Get NetAdapterRdma**命令。

1. 請確定 Mlnx 迷你連接埠和 Mlnx 匯流排驅動程式都是最新。 針對 Mellanox，使用至少卸除 42。 
2. 確認 Mlnx 迷你連接埠和匯流排驅動程式符合藉由檢查驅動程式版本透過裝置管理員。 在 系統裝置，可以找到匯流排驅動程式。 名稱開頭應該 Mellanox 連接 X 3 PRO VPI，如下列螢幕擷取畫面的網路介面卡內容中所示。

![網路介面卡內容](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. 請確定網路直接存取 (RDMA) 實體 NIC 和主機 vNIC 上啟用。
5. 請確定 vSwitch，會藉由檢查其 RDMA 功能建立透過實體介面卡。
6. 請檢查 EventViewer 系統記錄檔，並依來源 「 Hyper-V-VmSwitch 」 篩選。

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

若要確認您的 RDMA 設定額外的步驟，在 HYPER-V 伺服器上執行下列 Windows PowerShell 命令。


    Get-SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>取得 SmbClientNetworkInterface 預期結果

主機 vNIC 應該會顯示為具有 RDMA 功能，從 SMB 的檢視方塊。

![網路介面卡內容](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>取得 SmbClientNetworkInterface 非預期的結果

1. 請確定 Mlnx 迷你連接埠和 Mlnx 匯流排驅動程式都是最新。 針對 Mellanox，使用至少卸除 42。 
2. 確認 Mlnx 迷你連接埠和匯流排驅動程式符合藉由檢查驅動程式版本透過裝置管理員。 在 系統裝置，可以找到匯流排驅動程式。 名稱開頭應該 Mellanox 連接 X 3 PRO VPI，如下列螢幕擷取畫面的網路介面卡內容中所示。
3. 請確定網路直接存取 (RDMA) 實體 NIC 和主機 vNIC 上啟用。
4. 請確定 HYPER-V 虛擬交換器時，建立透過實體介面卡上，藉由檢查其 RDMA 功能。
5. 請檢查 EventViewer 記錄 「 SMB 用戶端 」**應用程式和服務 |Microsoft |Windows**。

--- 

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

您可以檢視網路介面卡服務品質\(QoS\)藉由執行下列 Windows PowerShell 命令的組態。

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Get-netadapterqos 預期結果

優先順序和流量類別應該會顯示根據您執行使用本指南的第一個組態步驟。

![服務優先順序和類別的品質](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-netadapterqos 非預期的結果

如果您的結果無法預期，請執行下列步驟。

1. 請確定實體網路介面卡支援的資料中心橋接\(DCB\)和 QoS
2. 請確定網路介面卡驅動程式是最新狀態。

--- 

## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

您可以使用下列 Windows PowerShell 命令來確認遠端節點的 IP 位址是 RDMA\-支援。

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>取得 SmbMultiChannelConnection 預期結果

遠端節點的 IP 位址會顯示為 RDMA 功能。

![RDMA 功能的遠端節點的 IP 位址](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>取得 SmbMultiChannelConnection 非預期的結果

如果您的結果無法預期，請執行下列步驟。

1. 請確定 ping 都行得通。
2. 請確定防火牆未封鎖 SMB 連線初始化。 具體來說，啟用防火牆規則 SMB 直接傳輸的連接埠 5445 對 iWARP 和 ROCE 的 445。

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

您可以使用下列命令來確認虛擬 NIC 您啟用針對 RDMA 報告的 RDMA\-支援 smb。

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>取得 SmbClientNetworkInterface 預期結果

已啟用 RDMA 的虛擬 NIC 必須被視為具有 RDMA 功能，smb。

![SMB 報告 Nic 是支援 RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>取得 SmbClientNetworkInterface 非預期的結果

如果您的結果無法預期，請執行下列步驟。

1. 請確定 ping 都行得通。
2. 請確定防火牆未封鎖 SMB 連線初始化。

--- 

## <a name="vstat-mellanox-specific"></a>vstat \(Mellanox 特定\)

如果您使用 Mellanox 網路介面卡，您可以使用**vstat**命令來確認透過聚合式乙太網路的 RDMA \(RoCE\) HYPER-V 節點上的版本。

### <a name="vstat-expected-results"></a>vstat 預期結果

在這兩個節點上的 RoCE 版本必須相同。 這也是確認兩個節點上的韌體版本是最新的好方法。

![RoCE 版本檢查的結果範例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat 非預期的結果

如果您的結果無法預期，請執行下列步驟。

1. 設定使用組 MlnxDriverCoreSetting 正確 RoCE 版本
2. 從 Mellanox 網站，請安裝最新的韌體。

--- 

## <a name="perfmon-counters"></a>Perfmon 計數器

您可以檢閱效能監視器來確認您的組態的 RDMA 活動中的計數器。

![效能監視器結果範例](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

--- 

## <a name="related-topics"></a>相關主題

- [具有單一網路介面卡的交集的 NIC 設定](cnic-single.md)
- [交集的 NIC 組合的 NIC 設定](cnic-datacenter.md)
- [交集的 NIC 的實體交換器組態](cnic-app-switch-config.md)

---
