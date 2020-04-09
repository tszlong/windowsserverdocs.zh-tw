---
title: 對於具有不屬於相同信任群組之虛擬機器的主伺服器，授權專案應具有不同的信任組名
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 22e1a3bdeb40c5440862b4931dda344756cc34a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857811"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>對於具有不屬於相同信任群組之虛擬機器的主伺服器，授權專案應具有不同的信任組名

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
*伺服器將接受複本虛擬機器的複寫要求，從與虛擬機器相同複寫標記相關聯的授權清單中的任何伺服器。*  
  
## <a name="impact"></a>**產生**  
*當虛擬機器接受來自屬於不同授權專案之主伺服器的複寫時，可能會有隱私權和安全性考慮。這會影響下列授權專案： \<授權專案的清單 >*  
  
## <a name="resolution"></a>**解決方法**  
*在主伺服器的授權專案中使用不同的標記，而這些虛擬機器不屬於相同的安全性群組。修改 Hyper-v 設定以設定複寫標記。*  
  


