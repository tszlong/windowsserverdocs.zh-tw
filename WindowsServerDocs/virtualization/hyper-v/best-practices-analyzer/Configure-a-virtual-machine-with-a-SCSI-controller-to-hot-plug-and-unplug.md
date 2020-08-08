---
title: 設定具有 SCSI 控制器的虛擬機器，以便能夠使用熱插即用的插入存放裝置
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0b914b2d250bbcb4c5e795ec778dde8db91613ec
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948459"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>設定具有 SCSI 控制器的虛擬機器，以便能夠使用熱插即用的插入存放裝置

>適用於：Windows Server 2016



*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題

*找到未設定 SCSI 控制器的虛擬機器。*

## <a name="impact"></a>影響

*您將無法對下列虛擬機器進行熱插拔或熱拔撥存放區：*

\<list of virtual machine names>

熱插拔或熱拔下存放區的功能，可讓您更輕鬆地管理虛擬機器的存放裝置需求，而不需要停機。 不需要 SCSI 控制器的虛擬機器必須關閉，您才能新增或移除存放裝置。

## <a name="resolution"></a>解決方法

*如果您不需要此虛擬機器的熱插即用或最忙碌的存放區，則不需要採取任何動作。否則，請關閉虛擬機器，並將 SCSI 控制器新增至設定。*

若要使用 SCSI 控制器來進行熱插即用和撥出存放裝置，客體作業系統必須執行目前版本的 integration services。



