---
title: nlbmgr
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ed11e702aeae66458f888e454c1bc1d1bc22630
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887079"
---
# <a name="nlbmgr"></a>nlbmgr

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

使用網路負載平衡管理員，您還可以設定及管理您的網路負載平衡叢集與所有叢集主機從單一電腦，以及您可以將複寫至其他主機的叢集設定。 您可以從命令列使用命令來啟動網路負載平衡管理員**nlbmgr.exe**，這會安裝在**systemroot\System32**資料夾。
## <a name="syntax"></a>語法
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/help|在命令提示字元顯示說明。|
|/noping|網路負載平衡管理員可防止 ping 之前嘗試連絡他們透過 Windows Management Instrumentation (WMI) 的主機。 如果您已在所有可用的網路介面卡上停用網際網路控制訊息通訊協定 (ICMP)，請使用此選項。 如果網路負載平衡管理員會嘗試連絡未提供的主機，您就會發生延遲，使用此選項時。|
|/hostlist <filename>|載入到網路負載平衡管理員在 filename 中指定的主機。|
|/autorefresh <interval>|會導致網路負載平衡管理員重新整理其主機和叢集的資訊每隔<interval>秒。 如果未不指定任何間隔，資訊就會重新整理每隔 60 秒。|
|/?|在命令提示字元顯示說明。|
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)

