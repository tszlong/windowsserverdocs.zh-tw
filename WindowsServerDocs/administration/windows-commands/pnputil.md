---
title: pnputil
description: 了解如何管理驅動程式存放區使用 pnputil.exe 公用程式。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5bde78d97be8def9f8594572869c34ef213db480
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862539"
---
# <a name="pnputil"></a>pnputil

Pnputil.exe 是命令列公用程式可供您管理驅動程式存放區。 您可以使用 Pnputil 來新增驅動程式套件、 移除驅動程式套件，並列出存放區中的驅動程式封裝。

## <a name="syntax"></a>語法

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-a|指定要新增的已識別的 INF 檔案。|
|-d|指定要刪除的已識別的 INF 檔案。|
|-e|指定要列舉所有第三方 INF 檔案。|
|-f|指定要強制刪除所識別的 INF 檔案。 不能搭配 **– i**參數。|
|-i|指定要安裝的已識別的 INF 檔案。 不能搭配 **-f**參數。|
|/?|在命令提示字元顯示說明。|


## <a name="examples"></a>範例

-   pnputil.exe-a:\usbcam\USBCAM。INF 加入 INF 檔案 USBCAM 所指定。INF
-   pnputil.exe-c:\drivers\*.inf c:\drivers\ 中加入所有的 INF 檔案
-   pnputil.exe-i-a:\usbcam\USBCAM。加入 INF，安裝指定的驅動程式。
-   pnputil.exe – e 列舉所有的協力廠商驅動程式。
-   pnputil.exe-d oem0.inf 刪除指定的。
-   pnputil.exe-f-d oem0.inf 強制刪除指定的 INF 檔案。

## <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[Popd](popd.md)