---
title: 針對 SR-IOV 設定的執行中虛擬機器數目不應超過虛擬機器可用的虛擬函式數目
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 3c58e980471284c950f41551b02b92bf74aca7cc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854611"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>針對 SR-IOV 設定的執行中虛擬機器數目不應超過虛擬機器可用的虛擬函式數目

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
*針對單一根目錄 i/o 虛擬化（SR-IOV）設定的執行中虛擬機器數目沒有足夠的可用虛擬函式。*  
  
## <a name="impact"></a>影響  
*下列虛擬機器的網路效能可能不佳：*  
   
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*請考慮在不需要 SR-IOV 虛擬函式的一或多部虛擬機器上停用 SR-IOV。*  
  


