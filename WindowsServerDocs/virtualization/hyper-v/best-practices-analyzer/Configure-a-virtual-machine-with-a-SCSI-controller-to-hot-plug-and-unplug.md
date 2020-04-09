---
title: 設定具有 SCSI 控制器的虛擬機器，以便能夠使用熱插即用的插入存放裝置
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: bd49a911d278a1f07fe9630f39798204d760dd88
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862151"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>設定具有 SCSI 控制器的虛擬機器，以便能夠使用熱插即用的插入存放裝置

>適用於︰Windows Server 2016


  
*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*找到未設定 SCSI 控制器的虛擬機器。*  
  
## <a name="impact"></a>影響  
  
*您將無法對下列虛擬機器進行熱插拔或熱拔撥存放區：*  
  
\<虛擬機器名稱清單 >  
  
熱插拔或熱拔下存放區的功能，可讓您更輕鬆地管理虛擬機器的存放裝置需求，而不需要停機。 不需要 SCSI 控制器的虛擬機器必須關閉，您才能新增或移除存放裝置。  
  
## <a name="resolution"></a>解析度  
  
*如果您不需要此虛擬機器的熱插即用或最忙碌的存放區，則不需要採取任何動作。否則，請關閉虛擬機器，並將 SCSI 控制器新增至設定。*  
  
若要使用 SCSI 控制器來進行熱插即用和撥出存放裝置，客體作業系統必須執行目前版本的 integration services。  
  


