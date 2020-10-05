---
title: 更新-ServerFiles
description: 更新-ServerFiles 的參考文章，會使用儲存在伺服器%Windir%\System32\RemInst 資料夾中的最新檔案，更新 REMINST 共用資料夾中的檔案。
ms.topic: reference
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1a984c789a309395dcee0d93a6ba0877c44aab22
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730591"
---
# <a name="update-serverfiles"></a>更新-ServerFiles

使用儲存在伺服器%Windir%\System32\RemInst 資料夾中的最新檔案，更新 REMINST 共用資料夾中的檔案。 為確保 Windows 部署服務安裝的有效性，您應該在每次伺服器升級、service pack 安裝或更新至 Windows 部署服務檔案之後，執行此命令一次。

## <a name="syntax"></a>語法

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|

## <a name="examples"></a>範例

若要更新檔案，請輸入下列其中一項：
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)