---
title: 確保足夠的實體磁碟空間時才可以使用虛擬機器可讓您使用動態擴充虛擬硬碟
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 09e481b99594ac543dadab2b60bf9b3f4c29e54b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883289"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>確保足夠的實體磁碟空間時才可以使用虛擬機器可讓您使用動態擴充虛擬硬碟

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
*一或多個虛擬機器使用動態擴充虛擬硬碟。*  
  
## <a name="impact"></a>影響  
*動態擴充虛擬硬碟需要裝載的磁碟區上的可用空間，以便將虛擬硬碟的寫入發生時可以配置的空間。如果已用完可用空間，可能會影響依賴的實體儲存體的任何虛擬機器。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*監視可用磁碟空間，以確保有足夠的空間可進行擴充。請考慮虛擬機器關機，並使用 HYPER-V Manager 中的 [編輯磁碟精靈]，將此虛擬機器的每個動態擴充虛擬硬碟轉換成固定大小虛擬硬碟。*  
  


