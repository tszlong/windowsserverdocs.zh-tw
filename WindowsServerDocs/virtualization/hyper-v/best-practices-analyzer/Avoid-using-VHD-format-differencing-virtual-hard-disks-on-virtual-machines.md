---
title: 請避免使用 VHD 格式的差異在生產環境中執行伺服器工作負載的虛擬機器上的虛擬硬碟
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: da908d00a6b5c48a61dad89e8c7b08cf80b4314c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819179"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>請避免使用 VHD 格式的差異在生產環境中執行伺服器工作負載的虛擬機器上的虛擬硬碟

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
*一或多個虛擬機器使用 VHD 格式差異虛擬硬碟。*  
  
## <a name="impact"></a>**影響**  
*VHD 格式差異虛擬硬碟可能會發生電源中斷時，遇到一致性問題。如果實體磁碟的.vhd 檔案中，當發生電源中斷正在修改執行磁區的不完整或不正確更新，可能會發生一致性問題。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*關閉虛擬機器和轉換的 VHD 格式的差異為 VHDX 格式虛擬硬碟鏈結或合併到固定虛擬硬碟鏈結。（採取 VHDX 格式有協助防止因電源故障所損毀的磁碟的可靠性機制）。不過，不會轉換虛擬硬碟可能連接到舊版的 Windows 在某個時間點。Windows 版本早於 Windows Server 2012 不支援 VHDX 格式。*  
  


