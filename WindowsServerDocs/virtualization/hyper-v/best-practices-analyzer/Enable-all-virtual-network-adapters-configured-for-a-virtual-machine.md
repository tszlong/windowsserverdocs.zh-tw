---
title: 啟用所有虛擬機器設定的虛擬網路介面卡
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fbb1ef5283f6ccf8dfa355a09a86040be80f53e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844229"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>啟用所有虛擬機器設定的虛擬網路介面卡

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*在虛擬機器，可能會停用一或多個網路介面卡。*  
  
## <a name="impact"></a>影響  
  
*下列虛擬機器可能沒有網路連線：*  
  
\<清單中的虛擬機器名稱 >  
  
## <a name="resolution"></a>解析度  
  
*若要啟用所有虛擬網路介面卡，在客體作業系統中使用裝置管理員。如果找不到需要的介面卡，使用 HYPER-V 管理員來移除虛擬機器。*  
  


