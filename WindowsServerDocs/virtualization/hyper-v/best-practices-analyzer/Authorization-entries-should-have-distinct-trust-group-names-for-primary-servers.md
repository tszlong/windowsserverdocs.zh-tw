---
title: 對於具有不屬於相同信任群組之虛擬機器的主伺服器，授權專案應具有不同的信任組名
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ab71e0e6562b73d81b871914fd0e9e76570c518e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366545"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>對於具有不屬於相同信任群組之虛擬機器的主伺服器，授權專案應具有不同的信任組名

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*伺服器將接受複本虛擬機器的複寫要求，從與虛擬機器相同複寫標記相關聯的授權清單中的任何伺服器。*  
  
## <a name="impact"></a>**產生**  
@no__t 0There 可能是隱私權和安全性考慮，而虛擬機器接受來自屬於不同授權專案之主伺服器的複寫。這會影響下列授權專案： @no__t 0list 授權專案 > *  
  
## <a name="resolution"></a>**解決方法**  
@no__t-在具有不屬於相同安全性群組的虛擬機器之主伺服器的授權專案中0Use 不同的標記。修改 Hyper-v 設定以設定複寫標記。 *  
  


