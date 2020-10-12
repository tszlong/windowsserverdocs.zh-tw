---
title: Hyper-v 的最佳做法分析程式
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 3747faa5-6e9f-499e-8a79-3fb9d73b6b92
ms.date: 8/16/2016
ms.openlocfilehash: c48f94eef1ceb77e80701adca9aeddfb4656e53c
ms.sourcegitcommit: 6931830a70c5849d8f884cdc7bd4f5afc1a00cce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2020
ms.locfileid: "91955813"
---
# <a name="best-practices-analyzer-for-hyper-v"></a>Hyper-v 的最佳做法分析程式

> 適用於：Windows Server 2016

在 Windows 管理中，*最佳做法*是專家定義在一般情況下最理想的伺服器設定方式的指導方針。 最佳做法違規（甚至是重要的）可能不會造成問題。 但是，它們可以指出可能導致效能不佳、可靠性不佳、未預期的衝突、增加安全性風險或其他潛在問題的伺服器設定。

最佳做法分析程式會使用以這些最佳作法為基礎的規則來掃描您的電腦，並報告結果。 每個最佳作法規則都包含如何符合規則的詳細資料。 本節記載這些規則，以協助您瞭解適用于 Hyper-v 的最佳作法，並在掃描發現不符合最佳做法的條件時，解讀結果。

如需最佳做法分析程式和掃描的詳細資訊，請參閱 [最佳做法分析](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283329(v=ws.11))程式。

## <a name="about-hyper-v"></a>關於 Hyper-V
Hyper-v 可讓您在一部實體電腦上同時執行多個作業系統，方法是在虛擬機器中執行每一個。 虛擬機器可協助您更有效率且更靈活地使用計算資源。 如需 Hyper-v 的詳細資訊，請參閱 [Windows Server 2016 上的 hyper-v](../Hyper-V-on-Windows-Server.md)。



