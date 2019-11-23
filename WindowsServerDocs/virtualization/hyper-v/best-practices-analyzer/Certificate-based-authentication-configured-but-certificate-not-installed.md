---
title: 以憑證為基礎的驗證已設定，但指定的憑證未安裝在複本伺服器或容錯移轉叢集節點上
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0b107a4760cc3470c7f80d53feef00a2f8f789c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365197"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>以憑證為基礎的驗證已設定，但指定的憑證未安裝在複本伺服器或容錯移轉叢集節點上

>適用於︰Windows Server 2016


  
*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|錯誤|  
|**類別**|設定|  

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題  
  
*Hyper-v 複本已設定用來提供以憑證為基礎之複寫的安全性憑證，並未安裝在複本伺服器上（或任何容錯移轉叢集節點）。*  
  
## <a name="impact"></a>影響  
  
*當叢集容錯移轉或移至另一個節點時，如果新節點也沒有安裝適當的憑證，Hyper-v 複寫就會暫停。這會影響下列節點：*  
  
\<的節點清單 >  
  
## <a name="resolution"></a>解析度  
  
*在複本伺服器上安裝已設定的憑證（以及容錯移轉叢集中所有相關聯的節點（如果有的話）。*  
  


