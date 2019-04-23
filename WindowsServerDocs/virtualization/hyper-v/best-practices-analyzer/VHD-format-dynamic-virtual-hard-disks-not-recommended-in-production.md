---
title: VHD 格式動態虛擬硬碟不建議用於生產環境中執行伺服器工作負載的虛擬機器
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 324a60a0-1d15-4ef2-9f17-23cbd2eb42ce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6acd27fa0efa27ba74c28e290c789edca599f66f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849879"
---
# <a name="vhd-format-dynamic-virtual-hard-disks-are-not-recommended-for-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>VHD 格式動態虛擬硬碟不建議用於生產環境中執行伺服器工作負載的虛擬機器

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
*一或多個虛擬機器使用 VHD 格式的動態擴充虛擬硬碟。*  
  
## <a name="impact"></a>**影響**  
*VHD 格式動態虛擬硬碟可能會發生電源中斷時，獲得一致性問題。如果實體磁碟的.vhd 檔案中，當發生電源中斷正在修改執行磁區的不完整或不正確更新，可能會發生一致性問題。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*關閉虛擬機器，並將 VHD 格式動態虛擬硬碟轉換為 VHDX 格式虛擬硬碟或固定的虛擬硬碟。（採取 VHDX 格式有協助防止因系統電源故障所損毀的磁碟的可靠性機制）。不過，不會轉換虛擬硬碟可能連接到舊版的 Windows 在某個時間點。Windows 版本早於 Windows Server 2012 不支援 VHDX 格式。*  
  


