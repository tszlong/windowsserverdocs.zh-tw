---
title: 使用虛擬光纖通道介面卡設定的虛擬機器應設定為以光纖通道為基礎的存放裝置的高可用性
description: 瞭解當一或多部虛擬機器缺少與光纖通道型存放裝置的高可用性連線時，該怎麼辦，因為這些虛擬機器是設定為只連接到一個主機匯流排介面卡 (HBA) 的虛擬光纖通道介面卡。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
ms.date: 8/16/2016
ms.openlocfilehash: 407db3e9e9f289deb438f67ad7e79c98cb2b914b
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845835"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>使用虛擬光纖通道介面卡設定的虛擬機器應設定為以光纖通道為基礎的存放裝置的高可用性

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|資訊|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*一或多部虛擬機器缺少與光纖通道型存放裝置的高可用性連線，因為這些虛擬機器設定的虛擬光纖通道介面卡，只會連線到一個 (HBA) 的主機匯流排介面卡。*

## <a name="impact"></a>**影響**
*主機匯流排介面卡失敗可能會封鎖存放裝置與虛擬機器之間的光纖通道連接。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方法**
*將虛擬機器中的另一個連線新增至主機匯流排介面卡，並設定客體作業系統中 (MPIO) 的多重路徑 i/o，以建立重複的光纖通道連接。*



