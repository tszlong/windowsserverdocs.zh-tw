---
title: pnputil
description: Pnputil 命令的參考文章，可新增驅動程式套件、移除驅動程式套件，以及列出驅動程式存放區中的驅動程式套件（使用 pnputil.exe 公用程式）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 01b8aee1aa4dfb85b590c9d4abbec471fc437da8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930240"
---
# <a name="pnputil"></a>pnputil

Pnputil.exe 是一種命令列公用程式，可讓您用來管理驅動程式存放區。 您可以使用此命令來新增驅動程式套件、移除驅動程式套件，以及列出存放區中的驅動程式套件。

## <a name="syntax"></a>語法

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| -a | 指定要新增已識別的 INF 檔案。 |
| -d | 指定刪除已識別的 INF 檔案。 |
| -E | 指定列舉所有協力廠商 INF 檔案。 |
| -f | 指定強制刪除已識別的 INF 檔案。 不能與 **– i**參數一起使用。 |
| -i | 指定安裝已識別的 INF 檔案。 不能與 **-f**參數一起使用。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

新增名為 USBCAM 的 INF 檔案。INF，請輸入：

```
pnputil.exe -a a:\usbcam\USBCAM.INF
```

若要新增位於 c:\drivers 中的所有 INF 檔案，請輸入：

```
pnputil.exe -a c:\drivers\*.inf
```

新增和安裝 USBCAM。INF 驅動程式，輸入：

```
pnputil.exe -i -a a:\usbcam\USBCAM.INF
```

若要列舉所有協力廠商驅動程式，請輸入：

```
pnputil.exe –e
```

若要刪除名為 oem0.inf 的 INF 檔案和驅動程式，請輸入：

```
pnputil.exe -d oem0.inf
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [popd 命令](popd.md)
