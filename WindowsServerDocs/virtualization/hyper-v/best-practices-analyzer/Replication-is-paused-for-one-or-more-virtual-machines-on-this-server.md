---
title: 一或多個虛擬機器已暫停複寫在此伺服器上
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 248b5fbdbfb54380e441d14cde6beaa9146ce800
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827769"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>一或多個虛擬機器已暫停複寫在此伺服器上

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|操作|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
*一或多個虛擬機器已暫停複寫。當暫停主要虛擬機器時，就會發生的任何變更將會累積，並後繼續複寫時將會傳送到複本虛擬機器。*  
  
## <a name="impact"></a>影響  
*只要複寫已暫停，累積在主要虛擬機器中發生的變更會耗用在主要伺服器上的可用磁碟空間。繼續複寫之後，可能會有大量暴增到複本伺服器的網路流量。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*確認預定暫停複寫。如果位址低磁碟空間或網路連線的複寫已暫停，繼續複寫，因為，在解決這些問題。*  
  


