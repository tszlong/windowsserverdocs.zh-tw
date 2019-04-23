---
title: vRSS 常見問題集
description: 本主題中，您會發現一些常被詢問問與答使用 vRSS。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3fafe6c39285e65a9d39a76cc6b652dac5c3efbd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840239"
---
# <a name="vrss-frequently-asked-questions"></a>vRSS 常見問題集

本主題中，您會發現一些常被詢問問與答使用 vRSS。

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>VRSS 搭配使用的實體網路介面卡的需求有哪些？

網路介面卡必須與虛擬機器佇列相容\(VMQ\) ，而且必須連結速度的 10 Gbps 以上。

如需詳細資訊，請參閱 <<c0> [ 計劃使用 vRSS](vrss-plan.md)。

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>VRSS 運作與超\-執行緒處理器核心？

資料分割 VRSS 和 VMQ 略過超\-執行緒處理器核心。

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>VRSS 運作的主機虛擬 Nic \(Vnic\)嗎？

是的。 使用 **-ManagementOS**參數，而不是虛擬機器\(VM\)名稱上**Set-vmnetworkadapter** Windows PowerShell 命令和**啟用 NetAdapterRss**主機 vNIC 上。

如需詳細資訊，請參閱 < [Windows PowerShell 命令，RSS 和 vRSS](vrss-wps.md)。

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>若要使用 vRSS 將 VM 需要在多少個邏輯處理器？

Vm 需要兩個或多個邏輯處理器\(LPs\)要能夠使用 vRSS。

如需詳細資訊，請參閱 <<c0> [ 計劃使用 vRSS](vrss-plan.md)。

## <a name="is-vrss-compatible-with-nic-teaming"></a>是 vRSS 與 NIC 小組相容？

是的。 如果您使用 NIC 小組，務必正確設定 VMQ 以搭配使用 NIC 小組設定。 如需 NIC 小組的部署和管理的詳細資訊，請參閱[NIC 小組](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>已啟用 vRSS，但是如何知道它是否正在運作？ 

您可以告訴 vRSS 正在開啟工作管理員，在您的 VM，並檢視虛擬處理器使用率。 如果有多個 VM 建立的連接，您可以看到多個核心使用率超過 0%。

因為單一 TCP 工作階段不能是負載平衡到多個邏輯處理器核心，您的 VM 必須可接收多個 TCP 工作階段之前，您可以觀察 vRSS 運作。

如果 VM 正在接收多個 TCP 工作階段，但不是會看到多個 LP 核心使用率超過 0%，請確定您已完成所有準備步驟 > 主題中[計劃使用 vRSS](vrss-plan.md)。

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>我正在檢視主機，看來並非所有處理器都已使用。 似乎其他處理器都被忽略。
  
檢查是否已啟用超執行緒。 VMQ 和 vRSS 專為略過超\-執行緒核心。

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>RSS 和 vRSS 上有不同的 Windows PowerShell 命令嗎？

是和否。 雖然您可以使用相同的命令在原生的主控件中的 RSS 及 RSS 在 Vm 中的，vRSS 也需要啟用實體 NIC 和 VM 和 vRSS 交換器連接埠上啟用 VMQ。

如需詳細資訊，請參閱 < [Windows PowerShell 命令，RSS 和 vRSS](vrss-wps.md)。

---
