---
title: 計劃 vRSS 使用
description: 您可以使用本主題適用於 Windows Server 2016 中使用 vRSS 準備您的虛擬機器與 HYPER-V 主機。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: e6558b00e87721d8ab81c84946a14745c4faa812
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133384"
---
# 計劃 vRSS 使用

>適用於：Windows Server (半年通道)、Windows Server 2016

在 Windows Server 2016 中，vRSS 預設為啟用，但是您必須準備您的環境，以允許在虛擬機器中正常運作的 vRSS \(VM\) 或在主機虛擬介面卡 \(vNIC\)。 在 Windows Server 2012 R2 中，預設已停用 vRSS。

當您計劃，並準備 vRSS 使用時，確保：

- 實體網路介面卡與虛擬機器佇列 \(VMQ\) 相容，並具有 10 gbps 連結速度或多個。
- VMQ 已啟用 HYPER-V 虛擬交換器連接埠和實體 NIC 上
- 沒有任何單一根 Input\ 輸出虛擬化 \(SR\-IOV\) 設定 vm。
- NIC 小組已正確設定。
- VM 有多個邏輯處理器 \(LPs\)。

>[!NOTE]
>vRSS 也會有 RSS 啟用任何主機 Vnic 的預設啟用。

以下是您需要完成這些步驟中準備的其他資訊。
  
1. **網路介面卡的容量**。 確認網路介面卡和虛擬機器佇列 \(VMQ\) 相容，且有 10 Gbps 或以上的連結速度。 如果連結速度小於 10 Gbps，HYPER-V 虛擬交換器會停用 VMQ 根據預設，即使它仍會顯示 VMQ 為已啟用的 Windows PowerShell 命令**Get NetAdapterVmq**結果中。 您可以使用它來驗證 VMQ 是啟用或停用的其中一種方法是使用命令**Get NetAdapterVmqQueue**。  如果已停用 VMQ，這個命令的結果會顯示已指派給 VM 或主機虛擬網路介面卡沒有 QueueID。 
  
2. **啟用 VMQ**。 確認 VMQ 已在主機電腦上啟用。 如果主機不支援 VMQ vRSS 無法運作。 您可以確認 VMQ 的啟用方式執行**Get vmswitch 供應**，然後尋找正在使用之虛擬交換器的介面卡。 接下來，執行**Get NetAdapterVmq** ，並確保配接器會顯示在結果中，並且已 VMQ 啟用。
  
3. **SR\ IOV 不存在**。 確認單一根 Input\ 輸出虛擬化 \(SR\-IOV\) 虛擬函式 \(VF\) 驅動程式不附加到 VM 網路介面。 您可以使用**Get NetAdapterSriov**命令來驗證。 如果 VF 驅動程式載入時，RSS 會使用此驅動程式的縮放比例設定而不是由 vRSS 設定。 如果 VF 驅動程式不支援 RSS，則會停用 vRSS。
  
4. **NIC 小組設定**。 如果您使用 NIC 小組，請務必正確設定 VMQ，若要使用的 NIC 小組的設定。 如需 NIC 小組部署和管理的詳細資訊，請參閱[NIC 小組](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

5. **Lp 數目**。 確認 VM 有多個邏輯處理器 \(LP\)。 vRSS 依賴 VM 中的 RSS 或載入的平行處理多個 Lp 餘額接收流量 HYPER-V 主機上。 您可以看到您的 VM 在主機中執行的 Windows PowerShell 命令**Get VMProcessor**有多少 Lp。 執行命令之後，您可以數目的 Lp 觀察計數資料行項目。

主機 vNIC 隨時都能存取所有的實體處理器中;若要設定主機 vNIC 使用特定的處理器數量，使用 [設定 **-BaseProcessorNumber**和 **-MaxProcessors**當您執行**組 NetAdapterRss** Windows PowerShell 命令。

---