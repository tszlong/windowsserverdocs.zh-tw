---
title: 設定您希望複本虛擬機器在容錯移轉時使用的容錯移轉 TCP/IP 設定
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d2db5fdedbe2f19c01b7dd172f18b6fec969e828
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862051"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>設定您希望複本虛擬機器在容錯移轉時使用的容錯移轉 TCP/IP 設定

>適用於︰Windows Server 2016
 
如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。
  
## <a name="issue"></a>問題  
*使用靜態 IP 位址設定的複本虛擬機器，應該設定為在容錯移轉時，使用其主要虛擬機器對應項的不同 IP 位址。*  
  
## <a name="impact"></a>影響  
*使用主要虛擬機器所支援之工作負載的用戶端，可能無法在容錯移轉之後連線到複本虛擬機器。此外，主要虛擬機器的原始 IP 位址在複本虛擬機器網路拓撲中將無效。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*使用 [Hyper-v 管理員] 來設定複本虛擬機器在容錯移轉時應使用的 IP 位址。*  
  


