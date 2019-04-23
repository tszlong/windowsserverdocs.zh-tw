---
title: Integration services 必須先安裝才能主要或複本虛擬機器可以使用替代的 IP 位址在容錯移轉之後
description: 線上版本，則此 Best Practices Analyzer 規則的詳細資訊的連結的文字。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 1ff8dbfd71655aee86ba7d0feac87ec2267a2171
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865509"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>Integration services 必須先安裝才能主要或複本虛擬機器可以使用替代的 IP 位址在容錯移轉之後

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
*參與複寫的虛擬機器可設定為使用特定的 IP 位址，如果發生容錯移轉，但前提是已在虛擬機器的客體作業系統中安裝整合服務。*  
  
## <a name="impact"></a>影響  
*在容錯移轉 （規劃、 未規劃或測試） 的事件中，複本虛擬機器將會上線與主要虛擬機器使用相同的 IP 位址。這項設定可能會造成連線問題。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*使用虛擬機器連線至虛擬機器中安裝 integration services。*  
  
自 Windows Server 2016 中，透過 Windows Update 提供的 Windows 虛擬機器的整合服務。 請確定這些虛擬機器會設定為接收 Windows 更新，以取得最新版本的 integration services。 Linux 核心現在包含 Linux integration services (LIS)，並更新新的版本，但較舊核心為基礎的 Linux 散發套件可能沒有最新的增強功能或修正。 如需詳細資訊，請參閱 <<c0> [ 支援的 Linux 和 FreeBSD 虛擬機器，在 Windows 上的 hyper-v](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。


