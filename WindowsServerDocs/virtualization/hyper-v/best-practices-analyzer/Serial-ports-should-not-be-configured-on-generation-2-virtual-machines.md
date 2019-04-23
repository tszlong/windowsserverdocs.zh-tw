---
title: 不應在第 2 代虛擬機器上設定序列埠
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 58c3fc5f975b85ce17ac5f7cca4930ec9e851e07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877379"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>不應在第 2 代虛擬機器上設定序列埠

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
*一或多個層代 2 部虛擬機器已設定的序列埠。*  
  
## <a name="impact"></a>**影響**  
*對下列虛擬機器，可能會影響效能：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*如果這是刻意設計的任何進一步的動作不是必要的。否則，請考慮使用 HYPER-V 管理員或 Windows PowerShell 來移除虛擬機器上的序列埠的連接字串。*  
  


