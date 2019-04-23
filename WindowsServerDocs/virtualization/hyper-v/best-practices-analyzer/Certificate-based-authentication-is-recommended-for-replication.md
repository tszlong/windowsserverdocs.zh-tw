---
title: 憑證型驗證被建議的複寫
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5bd131b48b009b43b6379f72f370b4179dea5a03
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873059"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>憑證型驗證被建議的複寫

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
*選取以進行複寫的一或多個虛擬機器會設定為 Kerberos 驗證。*  
  
## <a name="impact"></a>**影響**  
*從主要伺服器與複寫伺服器的複寫網路流量是未加密。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*如果另一種方法來執行加密，您可以忽略此。否則，請修改虛擬機器設定，來選擇憑證型驗證。*  
  


