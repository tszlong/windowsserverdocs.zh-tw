---
title: 規劃使用 vRSS
description: 您可以使用本主題來準備您的虛擬機器和 HYPER-V 主機在 Windows Server 2016 中使用 vRSS。
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850439"
---
# <a name="plan-the-use-of-vrss"></a>規劃使用 vRSS

>適用於：Windows Server （半年通道），Windows Server 2016

在 Windows Server 2016 中，vRSS 預設會啟用，不過，您必須備妥環境來允許虛擬機器中正常運作的 vRSS \(VM\)或主機虛擬介面卡上\(vNIC\)。 Windows Server 2012 r2 中預設已停用 vRSS。

當您計劃，並準備使用 vRSS 時，請確認：

- 實體網路介面卡已與虛擬機器佇列相容\(VMQ\)和具有連結速度的 10 Gbps 或更多。
- Hyper-v 和實體 NIC 上，啟用 VMQ\-V 虛擬交換器連接埠
- 沒有單一根目錄輸入\-輸出虛擬化\(SR\-SR-IOV\)已針對 VM 設定。
- NIC 小組的設定正確。
- VM 有多個邏輯處理器\(LPs\)。

>[!NOTE]
>啟用 rss 任何主機 Vnic 的預設值也會啟用 vRSS。

以下是您需要完成這些準備步驟的其他資訊。
  
1. **網路介面卡容量**。 確認網路介面卡與虛擬機器佇列相容\(VMQ\)和具有連結速度的 10 Gbps 或更多。 連結速度是否小於 10 Gbps，Hyper\-V 虛擬交換器預設會停用 VMQ，即使它仍然顯示 VMQ，如 Windows PowerShell 命令的結果中啟用**Get-netadaptervmq**。 您可以使用來確認 VMQ 已啟用或停用的一種方法是使用命令**Get-netadaptervmqqueue**。  如果 VMQ 已停用，此命令的結果會顯示已指派給 VM 或主機的虛擬網路介面卡沒有 QueueID。 
  
2. **啟用 VMQ**。 確認 VMQ 是否在主機電腦上啟用。 如果主機不支援 VMQ，則 vRSS 即無法運作。 您可以確認 VMQ 是否啟用執行**Get-vmswitch**和尋找使用的虛擬交換器的介面卡。 接下來，執行 **Get-NetAdapterVmq** ，確定介面卡顯示在結果且已啟用 VMQ。
  
3. **缺乏 SR\-SR-IOV**。 驗證單一根目錄輸入\-輸出虛擬化\(SR\-SR-IOV\)虛擬函式\(VF\)驅動程式未附加至 VM 網路介面。 您可以使用來確認這**Get-netadaptersriov**命令。 如果已載入 VF 驅動程式，RSS 會使用來自此驅動程式的調整設定，而不是由 vRSS。 如果 VF 驅動程式不支援 RSS，則會停用 vRSS。
  
4. **NIC 小組設定**。 如果您使用 NIC 小組，務必正確設定 VMQ 以搭配使用 NIC 小組設定。 如需 NIC 小組的部署和管理的詳細資訊，請參閱[NIC 小組](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

5. **數目 LPs**。 請確認 VM 有多個邏輯處理器\(LP\)。 在 VM 中的 RSS 或 HYPER-V 主機上流量進行負載平衡接收到多個 LPs 進行平行處理，必須藉助 vRSS。 您可以觀察您的 VM 已經執行 Windows PowerShell 命令的幾個 LPs **Get-vmprocessor**主應用程式中。 執行命令之後，您可以觀察 LPs 數目的計數資料行項目。

主機 vNIC 一律有權存取的所有實體處理器;若要設定主機 vNIC 使用特定數目的處理器，使用 設定 **-BaseProcessorNumber**並 **-MaxProcessors**當您執行**Set-netadapterrss**Windows PowerShell 命令。

---