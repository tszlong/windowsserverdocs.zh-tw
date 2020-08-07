---
title: 將虛擬機器設定為只有在客體作業系統支援時才使用 SR-IOV
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1939aa6e866eddcf5ac99a784332b65cd594ceb8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935506"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>將虛擬機器設定為只有在客體作業系統支援時才使用 SR-IOV

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題
*一或多部虛擬機器已設定為使用單一根目錄 i/o 虛擬化 (SR-IOV) ，但客體作業系統不支援 SR-IOV*

## <a name="impact"></a>影響
*SR-IOV 虛擬函式將不會配置給下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*在執行不支援 SR-IOV 之客體作業系統的所有虛擬機器上，停用 SR-IOV。*

只有部分64位 Windows 來賓才支援 SR-IOV。 如需詳細資訊，請參閱[世代和來賓的 hyper-v 功能相容性](../Hyper-V-feature-compatibility-by-generation-and-guest.md)。



