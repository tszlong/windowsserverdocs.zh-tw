---
title: 授權項目應該會有不同的信任與不屬於相同的信任群組的虛擬機器的主要伺服器的群組名稱
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5c5b1a6bf8ef0bbceb5dde6b28cd951f399fc5e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882959"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>授權項目應該會有不同的信任與不屬於相同的信任群組的虛擬機器的主要伺服器的群組名稱

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*伺服器會接受從同一個複寫標記做為虛擬機器相關聯的授權清單中的伺服器任一複本虛擬機器的複寫要求。*  
  
## <a name="impact"></a>**影響**  
*可能會有隱私權和安全性考量，以接受來自屬於不同的授權項目的的主要伺服器的複寫虛擬機器。這會影響下列授權項目：\<的授權項目清單 >*  
  
## <a name="resolution"></a>**解決方法**  
*授權項目中使用不同的標記，針對具有不屬於相同的安全性群組的虛擬機器的主要伺服器。修改 HYPER-V 設定 來設定複寫標記。*  
  


