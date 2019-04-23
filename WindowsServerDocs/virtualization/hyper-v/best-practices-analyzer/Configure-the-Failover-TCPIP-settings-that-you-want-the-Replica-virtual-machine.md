---
title: 設定您想要使用容錯移轉時，複本虛擬機器的容錯移轉 TCP/IP 設定
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c16fbc95c9d679611d57327992a6621d58d4e201
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855749"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>設定您想要使用容錯移轉時，複本虛擬機器的容錯移轉 TCP/IP 設定

>適用於：Windows Server 2016
 
如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。
  
## <a name="issue"></a>問題  
*複本虛擬機器設定為使用靜態 IP 位址應該設定為使用不同的 IP 位址，容錯移轉時，其主要虛擬機器對應項中。*  
  
## <a name="impact"></a>影響  
*使用主要虛擬機器所支援的工作負載的用戶端可能無法在容錯移轉之後連接到複本虛擬機器。此外，主要虛擬機器的原始 IP 位址不會有效複本虛擬機器的網路拓樸中。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*您可以使用 HYPER-V 管理員來設定複本虛擬機器應該容錯移轉時使用的 IP 位址。*  
  


