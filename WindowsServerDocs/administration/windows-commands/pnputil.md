---
title: pnputil
description: 瞭解如何使用 pnputil 公用程式來管理驅動程式存放區。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: f20c60bfd9ae33497dd356c7797b9fb1d2b51d18
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372286"
---
# <a name="pnputil"></a>pnputil

Pnputil 是一種命令列公用程式，可讓您用來管理驅動程式存放區。 您可以使用 Pnputil 來新增驅動程式套件、移除驅動程式套件，以及列出存放區中的驅動程式套件。

## <a name="syntax"></a>語法

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-a|指定要新增已識別的 INF 檔案。|
|-d.ddd...e|指定刪除已識別的 INF 檔案。|
|-e|指定列舉所有協力廠商 INF 檔案。|
|-f|指定強制刪除已識別的 INF 檔案。 不能與 **– i**參數一起使用。|
|-i|指定安裝已識別的 INF 檔案。 不能與 **-f**參數一起使用。|
|/?|在命令提示字元顯示說明。|


## <a name="examples"></a>範例

-   pnputil .exe-a a:\usbcam\USBCAM。INF 會新增 USBCAM 所指定的 INF 檔案。份
-   pnputil .exe-c：\ drivers\*.inf 會在 c:\drivers\ 中新增所有 INF 檔案
-   pnputil .exe-i-a a:\usbcam\USBCAM。INF 會新增並安裝指定的驅動程式。
-   pnputil-e 列舉所有協力廠商驅動程式。
-   pnputil： d oem0.inf 會刪除指定的。
-   pnputil-f-d oem0.inf 會強制刪除指定的 INF 檔案。

## <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[Popd](popd.md)