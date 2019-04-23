---
title: 若要使用 SR-IOV 只支援客體作業系統時，才的虛擬機器設定
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5c2acb21fe8b11e8f020c6d2ab1742116c23b28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833359"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>若要使用 SR-IOV 只支援客體作業系統時，才的虛擬機器設定

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
*一或多個虛擬機器設定為使用單一根目錄 I/O 虛擬化 (SR-IOV)，但客體作業系統不支援 SR-IOV*  
  
## <a name="impact"></a>影響  
*SR-IOV 虛擬函式不會配置到下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*所有執行客體作業系統不支援 SR-IOV 的虛擬機器上，停用 SR-IOV。*  
  
SR-IOV 只支援一些 64 位元的 Windows 客體。 如需詳細資訊，請參閱 < [HYPER-V 功能的相容性世代與客體各自](../Hyper-V-feature-compatibility-by-generation-and-guest.md)。  
  


