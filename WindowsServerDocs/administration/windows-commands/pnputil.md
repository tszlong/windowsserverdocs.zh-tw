---
title: pnputil
description: 瞭解如何使用 pnputil 公用程式來管理驅動程式存放區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 134e6ce4b1fc44450047de3287b7daac67da4b6a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837511"
---
# <a name="pnputil"></a>pnputil

Pnputil 是一種命令列公用程式，可讓您用來管理驅動程式存放區。 您可以使用 Pnputil 來新增驅動程式套件、移除驅動程式套件，以及列出存放區中的驅動程式套件。

## <a name="syntax"></a>語法

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-a|指定要新增已識別的 INF 檔案。|
|-d|指定刪除已識別的 INF 檔案。|
|-e|指定列舉所有協力廠商 INF 檔案。|
|-f|指定強制刪除已識別的 INF 檔案。 不能與 **– i**參數一起使用。|
|-i|指定安裝已識別的 INF 檔案。 不能與 **-f**參數一起使用。|
|/?|在命令提示字元顯示說明。|


## <a name="examples"></a>範例

-   pnputil .exe-a a:\usbcam\USBCAM。INF 會新增 USBCAM 所指定的 INF 檔案。份
-   pnputil .exe-a c:\drivers\*.inf 會在 c:\drivers\ 中新增所有 INF 檔案
-   pnputil .exe-i-a a:\usbcam\USBCAM。INF 會新增並安裝指定的驅動程式。
-   pnputil-e 列舉所有協力廠商驅動程式。
-   pnputil： d oem0.inf 會刪除指定的。
-   pnputil-f-d oem0.inf 會強制刪除指定的 INF 檔案。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

[Popd](popd.md)