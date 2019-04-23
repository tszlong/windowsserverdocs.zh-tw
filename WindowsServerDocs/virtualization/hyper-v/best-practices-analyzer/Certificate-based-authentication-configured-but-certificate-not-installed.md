---
title: 設定憑證型驗證，但複本伺服器或容錯移轉叢集節點上未安裝指定的憑證
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: feef30d2f798057e4bd8e53ebab240af6b1d25b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832939"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>設定憑證型驗證，但複本伺服器或容錯移轉叢集節點上未安裝指定的憑證

>適用於：Windows Server 2016


  
*如需最佳做法和掃描的詳細資訊，請參閱* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|組態|  

在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。

## <a name="issue"></a>問題  
  
*HYPER-V 複本，已設定為使用以憑證為基礎的複寫在複本伺服器 （或任何容錯移轉叢集節點） 上未安裝安全性憑證。*  
  
## <a name="impact"></a>影響  
  
*發生叢集容錯移轉或移動到另一個節點時，如果新的節點也沒有安裝適當的憑證，將會暫停 Hyper-v VM 複寫。這會影響下列節點：*  
  
\<節點清單 >  
  
## <a name="resolution"></a>解析度  
  
*安裝已設定的憑證上的複本伺服器 （和所有相關聯的節點，在容錯移轉叢集中，如果有的話）。*  
  


