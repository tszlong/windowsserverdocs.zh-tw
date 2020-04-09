---
title: 避免在生產環境中執行伺服器工作負載的虛擬機器上使用 VHD 格式差異虛擬硬碟
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a11959266db4c9f3da73123c41a211198f27b9a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857721"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>避免在生產環境中執行伺服器工作負載的虛擬機器上使用 VHD 格式差異虛擬硬碟

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*一或多部虛擬機器使用 VHD 格式的差異虛擬硬碟。*  
  
## <a name="impact"></a>**產生**  
*VHD 格式差異虛擬硬碟可能會在發生電源中斷時遇到一致性問題。如果實體磁片對 .vhd 檔案中的磁區執行了不完整或不正確的更新，但發生電源中斷時，就會發生一致性問題。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*關閉虛擬機器，並將 VHD 格式差異虛擬硬碟的鏈轉換成 VHDX 格式，或將該鏈合併到固定的虛擬硬碟。（VHDX 格式具有可靠性機制，可協助防止磁片因電源故障而損毀）。不過，如果虛擬硬碟可能會在某個時間點附加到舊版的 Windows，請勿將它轉換。Windows Server 2012 之前的 windows 版本不支援 VHDX 格式。*  
  


