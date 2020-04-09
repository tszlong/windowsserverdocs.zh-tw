---
title: 一或多個網路介面卡應設定為埠鏡像的來源
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 147fd00f-1440-44d1-94e3-3a8af63aa7ed
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: b2beb64629f95b04ddf1fa3f43634759899554c5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861861"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-source-for-port-mirroring"></a>一或多個網路介面卡應設定為埠鏡像的來源

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*一部或多部虛擬機器的網路介面卡設定為埠鏡像的目的地，但虛擬交換器上沒有對應的來源。*  
  
## <a name="impact"></a>**產生**  
*下列虛擬交換器和虛擬機器的埠鏡像將無法正確運作：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*使用 Windows PowerShell 或 Hyper-v 管理員來完成或更正埠鏡像設定。*  
  


