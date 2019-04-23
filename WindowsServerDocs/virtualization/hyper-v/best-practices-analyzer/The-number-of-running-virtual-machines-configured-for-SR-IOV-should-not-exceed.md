---
title: 執行設定的 SR-IOV 的虛擬機器的數目不應超過可用的虛擬機器的虛擬函式的數目
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e63df9283927437f9cfc62c052d83b07fe599b34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829749"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>執行設定的 SR-IOV 的虛擬機器的數目不應超過可用的虛擬機器的虛擬函式的數目

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
*沒有足夠的虛擬函式適用於執行 設定單一根目錄 I/O 虛擬化 (SR-IOV) 的虛擬機器數目。*  
  
## <a name="impact"></a>影響  
*網路效能可能會無法在下列虛擬機器上達到最佳：*  
   
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*請考慮停用不需要的 SR-IOV 虛擬函式的一個或多個虛擬機器上的 SR-IOV。*  
  


