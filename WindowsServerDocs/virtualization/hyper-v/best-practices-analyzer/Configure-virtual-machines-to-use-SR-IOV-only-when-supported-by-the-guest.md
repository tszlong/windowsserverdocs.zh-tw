---
title: 將虛擬機器設定為僅在客體作業系統支援時使用 SR-IOV
description: 瞭解當一或多部虛擬機器設定為使用單一根目錄 i/o 虛擬化 (SR-IOV) 時該怎麼辦，但客體作業系統不支援 SR-IOV。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
ms.date: 8/16/2016
ms.openlocfilehash: 2f919f3ed2708b9d388a5215d73e629dc93f9922
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846389"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>將虛擬機器設定為僅在客體作業系統支援時使用 SR-IOV

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*一或多部虛擬機器已設定為使用單一根目錄 i/o 虛擬化 (SR-IOV) ，但客體作業系統不支援 SR-IOV*

## <a name="impact"></a>影響
*SR-IOV 虛擬函式將不會配置給下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*在執行不支援 SR-IOV 之客體作業系統的所有虛擬機器上停用 SR-IOV。*

只有某些64位 Windows 來賓才支援 SR-IOV。 如需詳細資訊，請參閱 [hyper-v 功能與產生和來賓之間的相容性](../Hyper-V-feature-compatibility-by-generation-and-guest.md)。



