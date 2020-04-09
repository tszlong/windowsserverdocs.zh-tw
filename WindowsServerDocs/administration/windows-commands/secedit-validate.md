---
title: secedit：驗證
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9425f7a1fb821f4ecbaa7c1689c3baabbff6223
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834871"
---
# <a name="seceditvalidate"></a>secedit：驗證



驗證儲存在安全性範本（.inf 檔案）中的安全性設定。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Secedit /validate <configuration file name>  

```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|設定檔案名稱|必要。</br>指定要驗證之安全性範本的路徑和檔案名。|

## <a name="remarks"></a>備註

如果驗證安全性範本已損毀或設定不正確，則可協助您。

將不會套用不正確安全性範本。

記錄檔將不會更新。

在 Windows Server 2008 中，`Secedit /refreshpolicy` 已由 `gpupdate`取代。 如需有關如何重新整理安全性設定的詳細資訊，請參閱[Gpupdate](gpupdate.md)。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

在安全性範本上執行復原之後，您想要確認復原 inf 檔案 secRBKcontoso 是有效的。
```
Secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>其他參考資料

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)