---
title: 進度命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 841d9103354e3162489492ba7dd97e726b37d37d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370184"
---
# <a name="the-progress-command"></a>進度命令



顯示執行命令時的進度。 您可以使用 **/progress**搭配您執行的任何其他 WDSUTIL 命令。 請注意，您必須在**WDSUTIL**之後直接指定 **/verbose**和 **/progress** 。

## <a name="syntax"></a>語法

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>範例

若要初始化伺服器並顯示進度，請輸入：
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:"C:\RemoteInstall"
```