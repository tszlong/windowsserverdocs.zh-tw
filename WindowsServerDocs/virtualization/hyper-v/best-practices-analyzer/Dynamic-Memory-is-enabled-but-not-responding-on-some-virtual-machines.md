---
title: 動態記憶體是已啟用，但部分虛擬機器上沒有回應
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 95fd426929f3e2f6f01bc10b207a21a57f1d8370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887779"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>動態記憶體是已啟用，但部分虛擬機器上沒有回應

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
*一或多個虛擬機器發生問題所需的動態記憶體，在 客體作業系統的驅動程式。*  
  
## <a name="impact"></a>影響  
*客體作業系統，在下列虛擬機器可能無法執行，或可能會因為 HYPER-V 無法調整以動態方式來回應的記憶體需求的變更記憶體 unreliably 執行。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*如果在啟動虛擬機器，這是預期的行為。如果虛擬機器無法開機，請確定 integration services 已經升級為最新版本，而且在客體作業系統支援動態記憶體。*  
  
自 Windows Server 2016 中，透過 Windows Update 提供整合服務。 請確定虛擬機器會設定為接收更新，以取得最新版本的 integration services。  
  
動態記憶體會搭配特定版本的支援來賓。 請參閱[HYPER-V 動態記憶體概觀](https://technet.microsoft.com/library/hh831766.aspx)版本早於 Windows Server 2016 和 Windows 10。  
  


