---
title: 避免在生產環境中執行伺服器工作負載的虛擬機器上使用 VHD 格式的差異虛擬硬碟
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
ms.date: 8/16/2016
ms.openlocfilehash: 688dd348a7b00a32a50778cf0fb6350b37232f58
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747013"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>避免在生產環境中執行伺服器工作負載的虛擬機器上使用 VHD 格式的差異虛擬硬碟

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*一或多部虛擬機器使用 VHD 格式的差異虛擬硬碟。*

## <a name="impact"></a>**影響**
*如果發生電源中斷，VHD 格式的差異虛擬硬碟可能會遇到一致性問題。如果實體磁片對 .vhd 檔案中磁區的磁區執行了不完整或不正確的更新，但在發生電源中斷時，就會發生一致性問題。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方法**
*關閉虛擬機器，並將 VHD 格式差異虛擬硬碟的鏈轉換成 VHDX 格式，或將鏈合併到固定的虛擬硬碟。 (VHDX 格式的可靠性機制，可協助保護磁片免于損毀，因為電源故障 ) 。不過，如果可能在某個時間點將虛擬硬碟附加至舊版 Windows，請不要轉換該虛擬硬碟。Windows Server 2012 之前的 windows 版本不支援 VHDX 格式。*



