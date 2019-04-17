---
title: vRSS 常見問題集
description: 本主題中，您會發現一些常常見問題集和解答使用 vRSS。
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133814"
---
# vRSS 常見問題集

本主題中，您會發現一些常常見問題集和解答使用 vRSS。

## 實體網路介面卡，讓我搭配 vRSS 需求為何？

網路介面卡必須是與虛擬機器佇列 \(VMQ\) 相容，而且必須有 10 gbps 連結速度或更多。

如需詳細資訊，請參閱[計劃 vRSS 使用](vrss-plan.md)。

## 運作 vRSS hyper\ 執行緒處理器核心？

否。 VRSS 和 VMQ 忽略 hyper\ 執行緒處理器核心。

## 運作 vRSS 的主機虛擬 Nic \(vNICs\)？

是。 使用 **-ManagementOS**參數，而不是**組 VMNetworkAdapter** Windows PowerShell 命令上的虛擬機器 \(VM\) 名稱和**啟用 NetAdapterRss**主機 vNIC 上。

如需詳細資訊，請參閱[RSS 」 和 「 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

## 若要使用 vRSS 將 VM 需要在有多個邏輯處理器？

虛擬機器必須要能夠使用 vRSS 的兩個或多個邏輯處理器 \(LPs\)。

如需詳細資訊，請參閱[計劃 vRSS 使用](vrss-plan.md)。

## 是 vRSS 與 NIC 小組相容？

是。 如果您使用 NIC 小組，請務必正確設定 VMQ，若要使用的 NIC 小組的設定。 如需 NIC 小組部署和管理的詳細資訊，請參閱[NIC 小組](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

## vRSS 已啟用，但如何知道是否正在運作？ 

您將能夠告知 vRSS 正在運作中您的 VM 開啟工作管理員，並檢視虛擬處理器使用率。 如果有多個 vm 建立的連線，您可以看到多個核心上面 0%使用率。

由於在單一的 TCP 工作階段無法負載平衡跨多個邏輯處理器核心，您的 VM 必須接收多個 TCP 工作階段之前，您可以以觀察 vRSS 正常運作。

如果 VM 會收到多個 TCP 工作階段，但您不會看到一個以上的 LP 核心上方 0%使用率，請確定您已完成所有主題中的[計劃 vRSS 利用](vrss-plan.md)準備步驟。

## 我想要在主應用程式，而不是所有的處理器無法使用。 它看起來就像每一個會略過。
  
檢查是否已啟用超執行緒。 設計 VMQ 和 vRSS 略過 hyper\ 執行緒的核心。

## RSS 和 vRSS 上有不同的 Windows PowerShell 命令？

\ [是] 和 [否。 雖然在原生主機中的 RSS 和 RSS Vm 中的，您可以使用相同命令，vRSS 也會要求 VMQ，以啟用實體 nic-及 VM 與 vRSS 交換器連接埠上必須啟用。

如需詳細資訊，請參閱[RSS 」 和 「 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。

---
