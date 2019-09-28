---
title: 避免在生產環境中執行伺服器工作負載的虛擬機器上使用 VHD 格式差異虛擬硬碟
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 7b6bee685a72f8f9af2e16ffe7ac5cc1e1f22a4f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366429"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>避免在生產環境中執行伺服器工作負載的虛擬機器上使用 VHD 格式差異虛擬硬碟

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*一或多部虛擬機器使用 VHD 格式的差異虛擬硬碟。*  
  
## <a name="impact"></a>**產生**  
@no__t 0VHD-格式化差異虛擬硬碟可能會在發生電源中斷時遇到一致性問題。如果實體磁片對 .vhd 檔案中的磁區執行了不完整或不正確的更新，但發生電源中斷時，就會發生一致性問題。這會影響下列虛擬機器： *  
  
@no__t 0list 的虛擬機器 >  
  
## <a name="resolution"></a>**解決方法**  
@no__t-向下0Shut 虛擬機器，並將 VHD 格式差異虛擬硬碟的鏈轉換成 VHDX 格式，或將該鏈合併至固定的虛擬硬碟。（VHDX 格式具有可靠性機制，可協助防止磁片因電源故障而損毀）。不過，如果虛擬硬碟可能會在某個時間點附加到舊版的 Windows，請勿將它轉換。Windows Server 2012 之前的 windows 版本不支援 VHDX 格式。 *  
  


