---
title: Hyper-v 的最佳做法分析程式
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 3747faa5-6e9f-499e-8a79-3fb9d73b6b92
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 3c3bf66aca8c26d2f82f345accfbe02b751a138f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948499"
---
# <a name="best-practices-analyzer-for-hyper-v"></a>Hyper-v 的最佳做法分析程式

>適用於：Windows Server 2016

在 Windows 管理中，*最佳做法*是專家定義在一般情況下最理想的伺服器設定方式的指導方針。 最佳做法（甚至是重要違規）可能不一定會造成問題。 但是，它們可以指出可能導致效能不佳、可靠性不佳、非預期的衝突、安全性風險增加或其他潛在問題的伺服器設定。

最佳做法分析程式會使用以這些最佳做法為基礎的規則來掃描您的電腦，並報告結果。 每個最佳作法規則都包含如何遵守規則的詳細資料。 本節記載這些規則，以協助您瞭解適用于 Hyper-v 的最佳作法，並在掃描發現不符合最佳做法的條件時，解讀結果。

如需最佳做法分析程式與掃描的詳細資訊，請參閱[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。

## <a name="about-hyper-v"></a>關於 Hyper-V
Hyper-v 可讓您在一部實體電腦上執行多個作業系統，方法是在虛擬機器中執行每一個。 虛擬機器可協助您更有效率且更靈活地使用計算資源。 如需 Hyper-v 的詳細資訊，請參閱[Windows Server 2016 上的 hyper-v](../Hyper-V-on-Windows-Server.md)。



