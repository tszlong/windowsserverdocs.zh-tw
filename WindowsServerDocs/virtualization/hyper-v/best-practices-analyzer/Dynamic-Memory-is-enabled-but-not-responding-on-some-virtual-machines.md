---
title: 在某些虛擬機器上已啟用動態記憶體但沒有回應
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 98bf61e6e132db8c8a16bf719410aefb433a1d99
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861981"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>在某些虛擬機器上已啟用動態記憶體但沒有回應

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
*有一或多部虛擬機器發生在客體作業系統中動態記憶體所需的驅動程式問題。*  
  
## <a name="impact"></a>影響  
*下列虛擬機器中的客體作業系統可能無法執行，或可能會執行 unreliably，因為 Hyper-v 無法動態調整記憶體以回應記憶體需求的變更。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*如果虛擬機器正在開機，這就是預期的行為。如果虛擬機器未開機，請確定 integration services 已升級至最新版本，而且客體作業系統支援動態記憶體。*  
  
從 Windows Server 2016，integration services 會透過 Windows Update 傳遞。 請確定已將虛擬機器設定為接收更新，以取得最新版本的 integration services。  
  
動態記憶體適用于支援的來賓的特定版本。 如需比 Windows Server 2016 和 Windows 10 更舊的版本，請參閱[hyper-v 動態記憶體總覽](https://technet.microsoft.com/library/hh831766.aspx)。  
  


