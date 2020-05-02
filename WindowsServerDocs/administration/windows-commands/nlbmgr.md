---
title: nlbmgr
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59ec2ad6b4614f89f9c1c3cbda97d5283a2374bd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723745"
---
# <a name="nlbmgr"></a>nlbmgr

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用網路負載平衡管理員，您可以從單一電腦設定和管理您的網路負載平衡叢集和所有叢集主機，也可以將叢集設定複寫到其他主機。 您可以從命令列使用安裝在**systemroot\System32**資料夾中的**nlbmgr**命令，啟動「網路負載平衡管理員」。
## <a name="syntax"></a>語法
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
#### <a name="parameters"></a>參數

|        參數        |                                                                                                                                                                                                描述                                                                                                                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /help          |                                                                                                                                                                                   在命令提示字元顯示說明。                                                                                                                                                                                    |
|         /noping         | 防止「網路負載平衡管理員」在嘗試透過 Windows Management Instrumentation （WMI）連線之前 ping 主機。 如果您已在所有可用的網路介面卡上停用網際網路控制訊息通訊協定（ICMP），請使用此選項。 如果「網路負載平衡管理員」嘗試連線到無法使用的主機，您會在使用此選項時遇到延遲。 |
|  /hostlist<filename>   |                                                                                                                                                                將 filename 中指定的主機載入「網路負載平衡管理員」。                                                                                                                                                                 |
| /autorefresh<interval> |                                                                                                          使「網路負載平衡管理員」每<interval>秒重新整理其主機和叢集資訊。 如果未指定間隔，則會每隔60秒重新整理一次資訊。                                                                                                          |
|           /?            |                                                                                                                                                                                   在命令提示字元顯示說明。                                                                                                                                                                                    |

## <a name="additional-references"></a>其他參考
-   - [命令列語法關鍵](command-line-syntax-key.md)

