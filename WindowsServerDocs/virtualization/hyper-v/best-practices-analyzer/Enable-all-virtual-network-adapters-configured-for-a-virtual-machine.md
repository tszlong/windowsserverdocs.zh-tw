---
title: 啟用為虛擬機器設定的所有虛擬網路介面卡
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 79971ff503afcc1a087aced579e30989233c2b4a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861961"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>啟用為虛擬機器設定的所有虛擬網路介面卡

>適用於︰Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*虛擬機器中可能已停用一或多個網路介面卡。*  
  
## <a name="impact"></a>影響  
  
*下列虛擬機器可能沒有網路連線能力：*  
  
\<虛擬機器名稱清單 >  
  
## <a name="resolution"></a>解析度  
  
*使用客體作業系統中的 Device Manager 來啟用所有虛擬網路介面卡。如果不需要介面卡，請使用 Hyper-v 管理員將它從虛擬機器中移除。*  
  


