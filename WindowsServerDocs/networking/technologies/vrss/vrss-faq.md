---
title: vRSS 常見問題
description: 在本主題中，您會找到一些關於使用 vRSS 的常見問題和解答。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f0f3cb2bebf5aed31be0dda0c8e33a78aae94367
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871816"
---
# <a name="vrss-frequently-asked-questions"></a>vRSS 常見問題

在本主題中，您會找到一些關於使用 vRSS 的常見問題和解答。

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>搭配 vRSS 使用之實體網路介面卡的需求為何？

網路介面卡必須與虛擬機器佇列\(VMQ\)相容，而且必須具有 10 Gbps 或以上的連結速度。

如需詳細資訊，請參閱[規劃使用 vRSS](vrss-plan.md)。

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>VRSS 是否能與超\-執行緒處理器核心搭配使用？

資料分割 VRSS 和 VMQ 都會忽略超\-執行緒處理器核心。

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>VRSS 是否適用于主機虛擬 nic \(vnic\)？

是的。 在**VMNetworkAdapter** Windows PowerShell 命令上使用 **-ManagementOS**參數\(，\)而不是虛擬機器 VM 名稱，並在主機 vNIC 上**啟用-set-netadapterrss** 。

如需詳細資訊，請參閱[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>VM 需要有多少個邏輯處理器才能使用 vRSS？

Vm 需要兩個或多個\(邏輯\)處理器 LPs，才能夠使用 vRSS。

如需詳細資訊，請參閱[規劃使用 vRSS](vrss-plan.md)。

## <a name="is-vrss-compatible-with-nic-teaming"></a>VRSS 是否與 NIC 小組相容？

是的。 如果您使用的是 NIC 小組，請務必適當地設定 VMQ 以使用 NIC 小組設定。 如需有關 NIC 小組部署和管理的詳細資訊，請參閱[nic](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)小組。

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>已啟用 vRSS，但我要如何知道它是否正常運作？ 

您可以藉由在您的 VM 中開啟工作管理員，並查看虛擬處理器使用率，來告訴 vRSS 是否正常運作。 如果 VM 有多個建立的連線，您可以看到高於 0% 使用率的一個以上的核心。

因為單一 TCP 會話無法跨多個邏輯處理器核心進行負載平衡，所以您的 VM 必須接收多個 TCP 會話，才能觀察 vRSS 是否正常運作。

如果 VM 接收到多個 TCP 會話，但您沒有看到超過一個以上的 LP 核心超過 0% 使用率，請確定您已完成[規劃使用 vRSS](vrss-plan.md)主題中的所有準備步驟。

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>我正在查看主機，而不是所有的處理器都在使用中。 似乎其他處理器都被忽略。
  
檢查是否已啟用超執行緒。 VMQ 和 vRSS 都是設計來略過\-超執行緒核心。

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>RSS 和 vRSS 是否有不同的 Windows PowerShell 命令？

是和否。 雖然您在原生主機和 Vm 的 RSS 中同時使用相同的命令，但 vRSS 也需要在實體 NIC 上啟用 VMQ，並在交換器埠上啟用 VM 和 vRSS。

如需詳細資訊，請參閱[RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

---
