---
title: 應在虛擬機器中啟用顯示介面卡，以提供影片功能
description: 瞭解當虛擬機器中的 Microsoft 虛擬機器匯流排影片裝置可能已停用時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
ms.date: 8/16/2016
ms.openlocfilehash: 415d8c59b789ebdd77bfd16840926c55007749b9
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846381"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>應在虛擬機器中啟用顯示介面卡，以提供影片功能

>適用於：Windows Server 2016



*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*Microsoft 虛擬機器匯流排影片裝置可能會在虛擬機器中停用。*

Microsoft 虛擬機器匯流排影片裝置是已針對 Hyper-v 虛擬機器優化的虛擬視訊卡。 當虛擬機器未設定為使用 Microsoft 虛擬機器匯流排影片裝置時，就會使用舊版視訊卡。 Microsoft 虛擬機器匯流排影片裝置的執行效果優於舊版視訊卡。

## <a name="impact"></a>影響

*下列虛擬機器的影片效能將會降級：*

\<list of virtual machine names>

## <a name="resolution"></a>解決方案

*使用客體作業系統中的裝置管理員來啟用 Microsoft 虛擬機器匯流排影片裝置。*

使用裝置管理員所需的步驟會視作業系統而有所不同。 如需相關指示，請參閱客體作業系統中的說明。



