---
title: ServerFiles 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 93eeb0deaa527921db35f4ab955d2ccc46b57d7a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385855"
---
# <a name="the-update-serverfiles-command"></a>ServerFiles 命令



使用儲存在伺服器%Windir%\System32\RemInst 資料夾中的最新檔案，更新 REMINST 共用資料夾中的檔案。 若要確保 Windows 部署服務安裝的有效性，您應該在每次伺服器升級後執行此命令一次，Service Pack 安裝，或更新為 Windows 部署服務的檔案。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[/Server： \<Server 名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|

## <a name="BKMK_examples"></a>典型

若要更新檔案，請輸入下列其中一項：
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)