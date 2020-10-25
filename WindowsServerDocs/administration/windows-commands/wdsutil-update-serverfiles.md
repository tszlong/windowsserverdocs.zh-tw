---
title: Update-ServerFiles
description: 更新-ServerFiles 的參考文章，會使用儲存在伺服器%Windir%\System32\RemInst 資料夾中的最新檔案，更新 REMINST 共用資料夾中的檔案。
ms.topic: reference
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7684dfb694ac6814d00c91363d6573be5cf7be7f
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524893"
---
# <a name="update-serverfiles"></a>Update-ServerFiles

使用儲存在伺服器%Windir%\System32\RemInst 資料夾中的最新檔案，更新 REMINST 共用資料夾中的檔案。 為確保 Windows 部署服務安裝的有效性，您應該在每次伺服器升級、service pack 安裝或更新至 Windows 部署服務檔案之後，執行此命令一次。

## <a name="syntax"></a>語法

```
wdsutil [Options] /Update-ServerFiles [/Server:<Server name>]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|

## <a name="examples"></a>範例

若要更新檔案，請輸入下列其中一項：
```
wdsutil /Update-ServerFiles
wdsutil /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)