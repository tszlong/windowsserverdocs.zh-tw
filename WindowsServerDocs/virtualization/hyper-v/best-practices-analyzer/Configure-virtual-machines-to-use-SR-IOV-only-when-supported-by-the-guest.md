---
title: 將虛擬機器設定為只有在客體作業系統支援時才使用 SR-IOV
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0634b10d1ffa81d875a7b90c9a8eadcddd52b4e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862011"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>將虛擬機器設定為只有在客體作業系統支援時才使用 SR-IOV

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*一或多部虛擬機器已設定為使用單一根目錄 i/o 虛擬化（SR-IOV），但客體作業系統不支援 SR-IOV*  
  
## <a name="impact"></a>影響  
*SR-IOV 虛擬函式將不會配置給下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*在執行不支援 SR-IOV 之客體作業系統的所有虛擬機器上，停用 SR-IOV。*  
  
只有部分64位 Windows 來賓才支援 SR-IOV。 如需詳細資訊，請參閱[世代和來賓的 hyper-v 功能相容性](../Hyper-V-feature-compatibility-by-generation-and-guest.md)。  
  


