---
title: 更新-ServerFiles
description: ServerFiles 的參考文章，它會使用儲存在伺服器%Windir%\System32\RemInst 資料夾中的最新檔案，更新 REMINST 共用資料夾中的檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79b9332d5962c7e3c50ea3d7c71f33111a0e74b8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930136"
---
# <a name="update-serverfiles"></a>更新-ServerFiles

使用儲存在伺服器%Windir%\System32\RemInst 資料夾中的最新檔案，更新 REMINST 共用資料夾中的檔案。 若要確保 Windows 部署服務安裝的有效性，您應該在每次伺服器升級後執行此命令一次，Service Pack 安裝，或更新為 Windows 部署服務的檔案。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|

## <a name="examples"></a>範例

若要更新檔案，請輸入下列其中一項：
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)