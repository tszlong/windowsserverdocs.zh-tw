---
title: bitsadmin util 和版本
description: 適用于 bitsadmin util 和 version 的 Windows 命令主題，其會顯示 BITS 服務的版本。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 087cc1033166ab93e7496caaa7335433cafd6249
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848831"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util 和版本

顯示 BITS 服務的版本（例如，2.0）。

**BITSAdmin 1.5 和更早版本**：不支援。

## <a name="syntax"></a>語法

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>備註

**Verbose**參數會執行下列動作：
-   顯示每個位相關 DLL 的檔案版本
-   確認可以啟動 BITS 服務
-   顯示位群組原則值（僅限 Windows Vista）

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例是 BITS 服務的版本。
```
C:\>bitsadmin /Util /Version
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)