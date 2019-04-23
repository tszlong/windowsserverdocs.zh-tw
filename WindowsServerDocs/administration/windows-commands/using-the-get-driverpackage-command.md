---
title: 使用 get DriverPackage 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6574177f1f0a8ead0cc2fa596380eb2c980f8d1f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888519"
---
# <a name="using-the-get-driverpackage-command"></a>使用 get DriverPackage 命令



顯示伺服器上的驅動程式套件的相關資訊。

## <a name="syntax"></a>語法

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[/ 伺服器：\<伺服器名稱 >]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
|[/DriverPackage:\<Name>]|指定驅動程式套件，以顯示名稱。|
|[/PackageId:\<ID>]|指定 Windows 部署服務的識別碼的驅動程式套件來顯示。 如果驅動程式套件不能唯一識別名稱，您必須指定識別碼。|
|[] / [顯示: {驅動程式 | 檔案 | 所有}]|表示要顯示 （如果有指定） 的資訊。 如果 **/ 顯示**未指定，預設值是要傳回的驅動程式套件中繼資料。 **驅動程式**顯示封裝中的所有驅動程式。 **檔案**顯示封裝中的檔案清單。 **所有**顯示驅動程式、 檔案和中繼資料。|

## <a name="BKMK_examples"></a>範例

若要檢視驅動程式套件的相關資訊，請輸入下列其中一項：
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)