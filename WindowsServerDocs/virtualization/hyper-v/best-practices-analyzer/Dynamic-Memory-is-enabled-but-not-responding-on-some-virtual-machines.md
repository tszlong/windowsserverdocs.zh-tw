---
title: 動態記憶體已啟用，但在某些虛擬機器上沒有回應
description: 瞭解當一或多部虛擬機器遇到客體作業系統動態記憶體所需的驅動程式問題時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
ms.date: 8/16/2016
ms.openlocfilehash: 2fbf600e2fa06f01ddbf3ad3dafc02ded25dc6fc
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846113"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>動態記憶體已啟用，但在某些虛擬機器上沒有回應

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
*有一或多部虛擬機器遇到在客體作業系統中動態記憶體所需的驅動程式問題。*

## <a name="impact"></a>影響
*下列虛擬機器中的客體作業系統可能無法執行，或可能執行 unreliably，因為 Hyper-v 無法動態調整記憶體以回應記憶體需求的變更。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*如果虛擬機器正在開機，這就是預期的行為。如果虛擬機器未啟動，請確定 integration services 已升級為最新版本，而且客體作業系統支援動態記憶體。*

從 Windows Server 2016，integration services 會透過 Windows Update 傳遞。 確定已將虛擬機器設定為接收更新，以取得最新版本的 integration services。

動態記憶體適用于特定版本的支援來賓。 如需 Windows Server 2016 和 Windows 10 之前的版本，請參閱 [hyper-v 動態記憶體總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831766(v=ws.11)) 。