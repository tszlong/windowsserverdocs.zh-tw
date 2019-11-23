---
title: 第2代虛擬機器上不應設定序列埠
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8a8c15076921efa0e1e791a18c6a45ea1bf27b0e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364734"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>第2代虛擬機器上不應設定序列埠

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|設定|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*一或多個第2代虛擬機器已設定序列埠。*  
  
## <a name="impact"></a>**產生**  
*下列虛擬機器的效能可能會受到影響：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*如果這是故意的，則不需要採取進一步的動作。否則，請考慮使用 Hyper-v 管理員或 Windows PowerShell，從虛擬機器上的序列埠移除連接字串。*  
  


