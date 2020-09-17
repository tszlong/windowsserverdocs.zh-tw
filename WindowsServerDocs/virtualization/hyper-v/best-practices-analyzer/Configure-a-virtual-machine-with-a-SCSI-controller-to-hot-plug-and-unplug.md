---
title: 設定具有 SCSI 控制器的虛擬機器，以存取熱插即用的插即用存放區
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
ms.date: 8/16/2016
ms.openlocfilehash: 9f0cdb47206c89c4da66786e8dfd118e6a05be81
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746973"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>設定具有 SCSI 控制器的虛擬機器，以存取熱插即用的插即用存放區

>適用於：Windows Server 2016



*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*發現未使用 SCSI 控制器設定的虛擬機器。*

## <a name="impact"></a>影響

*您將無法存取下列虛擬機器的最忙碌隨插即用存放區：*

\<list of virtual machine names>

熱插即用或熱插即用存放裝置的能力，可讓您更輕鬆地管理虛擬機器的儲存需求，而不需要停機。 您必須關閉沒有 SCSI 控制器的虛擬機器，才能新增或移除存放裝置。

## <a name="resolution"></a>解決方案

*如果您不需要為此虛擬機器進行熱插即用隨插即用的儲存體，則不需要採取任何動作。否則，請關閉虛擬機器，並將 SCSI 控制器新增至設定。*

若要將 SCSI 控制器用於熱插即用的插即用存放區，客體作業系統必須執行目前版本的 integration services。



