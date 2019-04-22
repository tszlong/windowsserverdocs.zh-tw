---
title: 更新 ServerFiles 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ec96e2ba9aea14ed9a203dabbb697187736b33a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817439"
---
# <a name="the-update-serverfiles-command"></a>更新 ServerFiles 命令



使用最新的檔案儲存在伺服器的 %Windir%\System32\RemInst 資料夾中，以更新 REMINST 共用資料夾中的檔案。 若要確保您的 Windows 部署服務安裝的有效性，您應該執行此命令一次之後每個伺服器升級，安裝 service pack 或更新, Windows 部署服務的檔案。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[/ 伺服器：\<伺服器名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|

## <a name="BKMK_examples"></a>範例

若要更新的檔案，輸入下列其中一項：
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)