---
title: nlbmgr
description: Nlbmgr 命令的參考文章，可協助您使用網路負載平衡管理員，從單一電腦設定和管理您的網路負載平衡叢集和所有叢集主機。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19aca0285eca202b4e43a15b8e880f3672f0c846
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956710"
---
# <a name="nlbmgr"></a>nlbmgr

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用網路負載平衡管理員，從單一電腦設定和管理您的網路負載平衡叢集和所有叢集主機。 您也可以使用此命令將叢集設定複寫到其他主機。

您可以從命令列使用安裝在**systemroot\System32**資料夾中的命令**nlbmgr.exe**來啟動「網路負載平衡管理員」。

## <a name="syntax"></a>語法

```
nlbmgr [/noping][/hostlist <filename>][/autorefresh <interval>][/help | /?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /noping | 防止「網路負載平衡管理員」在嘗試透過 Windows Management Instrumentation （WMI）連線之前 ping 主機。 如果您已在所有可用的網路介面卡上停用網際網路控制訊息通訊協定（ICMP），請使用此選項。 如果「網路負載平衡管理員」嘗試連線到無法使用的主機，您會在使用此選項時遇到延遲。 |
| /hostlist`<filename>` | 將 filename 中指定的主機載入到網路負載平衡管理員。 |
| /autorefresh`<interval>` | 導致網路負載平衡管理員每秒重新整理其主機和叢集資訊 `<interval>` 。 如果未指定間隔，則會每隔60秒重新整理一次資訊。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [NetworkLoadBalancingClusters Cmdlet 參考](/powershell/module/networkloadbalancingclusters)
