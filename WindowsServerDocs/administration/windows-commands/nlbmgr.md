---
title: nlbmgr
description: Nlbmgr 命令的參考文章，可協助您使用網路負載平衡管理員，從單一電腦設定和管理網路負載平衡叢集和所有叢集主機。
ms.topic: reference
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dc3f1af169fc9510d95c348436a43036eee8cbb2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635754"
---
# <a name="nlbmgr"></a>nlbmgr

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用網路負載平衡管理員，從單一電腦設定及管理您的網路負載平衡叢集和所有叢集主機。 您也可以使用此命令，將叢集設定複寫至其他主機。

您可以使用安裝在**systemroot\System32**資料夾中的命令**nlbmgr.exe**，從命令列啟動「網路負載平衡管理員」。

## <a name="syntax"></a>語法

```
nlbmgr [/noping][/hostlist <filename>][/autorefresh <interval>][/help | /?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /noping | 防止網路負載平衡管理員在嘗試透過 Windows Management Instrumentation (WMI) 之前，先 ping 主機。 如果您已在所有可用的網路介面卡上停用 (ICMP) 的網際網路控制訊息通訊協定，請使用此選項。 如果 [網路負載平衡管理員] 嘗試聯繫無法使用的主機，您將會在使用此選項時遇到延遲。 |
| /hostlist `<filename>` | 將檔案名中指定的主機載入到網路負載平衡管理員。 |
| /autorefresh `<interval>` | 導致網路負載平衡管理員每秒重新整理其主機和叢集資訊 `<interval>` 。 如果未指定間隔，則會每隔60秒重新整理一次資訊。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [NetworkLoadBalancingClusters Cmdlet 參考](/powershell/module/networkloadbalancingclusters)
