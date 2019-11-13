---
title: 建議以憑證為基礎的驗證進行複寫
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0eac99fddd8bbc6dc585931cd25f2a440be16c76
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365184"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>建議以憑證為基礎的驗證進行複寫

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
*為複寫選取的一或多部虛擬機器已設定 Kerberos 驗證。*  
  
## <a name="impact"></a>**產生**  
*從主伺服器傳送到複寫伺服器的複寫網路流量未加密。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*如果正在使用另一個方法來執行加密，您可以忽略此情況。否則，請修改虛擬機器設定，以選擇以憑證為基礎的驗證。*  
  


