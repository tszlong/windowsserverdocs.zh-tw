---
title: 以虛擬光纖通道介面卡設定的虛擬機器應設定為以光纖通道為基礎的儲存體的高可用性
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: e04b52fc98fd79024970ed525e902132d97701e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855001"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>以虛擬光纖通道介面卡設定的虛擬機器應設定為以光纖通道為基礎的儲存體的高可用性

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|資訊|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。
  
## <a name="issue"></a>**問題**  
*一部或多部虛擬機器缺少與光纖通道型存放裝置的高可用性連線，因為這些虛擬機器設定的虛擬光纖通道介面卡僅連接到一個主機匯流排介面卡（HBA）。*  
  
## <a name="impact"></a>**產生**  
*主機匯流排介面卡失敗可能會封鎖存放裝置與虛擬機器之間的光纖通道連線。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*新增從虛擬機器到主機匯流排介面卡的另一個連線，並在客體作業系統中設定多重路徑 i/o （MPIO），以建立多餘的光纖通道連線。*  
  


