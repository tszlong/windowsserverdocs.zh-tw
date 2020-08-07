---
title: 規劃使用 vRSS
description: 您可以使用本主題來準備您的虛擬機器和 Hyper-v 主機，以便在 Windows Server 2016 中使用 vRSS。
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/04/2018
ms.openlocfilehash: 9457a1763f92e7f2571040c1c6e8e323d96ee598
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951937"
---
# <a name="plan-the-use-of-vrss"></a>規劃使用 vRSS

>適用於：Windows Server (半年度管道)、Windows Server 2016

在 Windows Server 2016 中，預設會啟用 vRSS，不過，您必須準備環境，讓 vRSS 能夠在虛擬機器 \( VM \) 或主機虛擬配接器 vNIC 上正確運作 \( \) 。 在 Windows Server 2012 R2 中，預設會停用 vRSS。

當您規劃並準備使用 vRSS 時，請確定：

- 實體網路介面卡與虛擬機器佇列 \( VMQ 相容 \) ，且連結速度為 10 Gbps 或更多。
- 已在實體 NIC 和 Hyper-v \- 虛擬交換器埠上啟用 VMQ
- 沒有為 VM 設定單一根輸入 \- 輸出虛擬化 \( sr-iov \- \) 。
- 已正確設定 NIC 小組。
- VM 有多個邏輯處理器 \( LPs \) 。

>[!NOTE]
>對於已啟用 RSS 的任何主機 Vnic，預設也會啟用 vRSS。

以下是完成這些準備步驟所需的其他資訊。

1. **網路介面卡容量**。 請確認網路介面卡與虛擬機器佇列 \( VMQ 相容 \) ，且連結速度為 10 Gbps 或更多。 如果連結速度小於 10 Gbps，則 \- 根據預設，Hyper-v 虛擬交換器會停用 vmq，即使在 Windows PowerShell 命令**get-netadaptervmq**的結果中，仍顯示已啟用的 vmq 也一樣。 您可以使用下列方法來確認 VMQ 已啟用或停用：使用命令**get-netadaptervmqqueue**。  如果 VMQ 已停用，則此命令的結果會顯示未將 QueueID 指派給 VM 或主機虛擬網路介面卡。

2. **啟用 VMQ**。 確認 VMQ 是否在主機電腦上啟用。 如果主機不支援 VMQ，vRSS 就無法運作。 您可以藉由執行「**取得-VMSwitch** 」並尋找虛擬交換器正在使用的介面卡，來確認已啟用 VMQ。 接下來，執行 **Get-NetAdapterVmq**，確定介面卡顯示在結果且已啟用 VMQ。

3. **缺少 SR \-SR-IOV**。 確認單一根輸入 \- 輸出虛擬化 \( sr-iov 虛擬函式 \- \) \( VF \) 驅動程式未連接至 VM 網路介面。 您可以使用**get-netadaptersriov**命令來確認這一點。 如果您已載入 VF 驅動程式，RSS 會使用此驅動程式的調整設定，而不是由 vRSS 設定的值。 如果 VF 驅動程式不支援 RSS，則會停用 vRSS。

4. **NIC**小組設定。 如果您使用的是 NIC 小組，請務必適當地設定 VMQ 以使用 NIC 小組設定。 如需有關 NIC 小組部署和管理的詳細資訊，請參閱[nic](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)小組。

5. **LPs 的數目**。 確認 VM 有一個以上的邏輯處理器 \( LP \) 。 vRSS 依賴 VM 或 Hyper-v 主機上的 RSS，以負載平衡接收到多個 LPs 的流量以進行平行處理。 您可以在主機中執行 Windows PowerShell 命令**VMProcessor** ，觀察 VM 有多少 LPs。 執行命令之後，您可以觀察 LPs 數目的 Count 資料行專案。

主機 vNIC 一律可以存取所有實體處理器;若要將主機 vNIC 設定為使用特定數目的處理器，請在執行**Set-netadapterrss** Windows PowerShell 命令時，使用**BaseProcessorNumber**和 **-MaxProcessors**的設定。

---