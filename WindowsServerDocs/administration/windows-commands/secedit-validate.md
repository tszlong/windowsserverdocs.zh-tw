---
title: Secedit： 驗證
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cca64f6b2904ed11f6b45e316c8e4da0093c373e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877909"
---
# <a name="seceditvalidate"></a>Secedit： 驗證



驗證安全性範本 （.inf 檔案） 中儲存的安全性設定。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Secedit /validate <configuration file name>  

```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|組態檔名稱|必要。</br>指定要驗證的安全性範本的路徑和檔案名稱。|

## <a name="remarks"></a>備註

驗證安全性範本可協助您如果其中一個已毀損，或未經正常管道報廢設定。

無效的安全性範本將不會套用。

將不會更新記錄檔。

在 Windows Server 2008、windows`Secedit /refreshpolicy`已取代為`gpupdate`。 如需如何重新整理的安全性設定資訊，請參閱[Gpupdate](gpupdate.md)。

## <a name="BKMK_Examples"></a>範例

安全性範本上執行回復之後，您想要確認回復 inf 檔案、 secRBKcontoso.inf，有效。
```
Secedit /validate secRBKcontoso.inf
```

#### <a name="additional-references"></a>其他參考資料

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   [命令列語法關鍵](command-line-syntax-key.md)