---
title: 命令的進度
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5fff31c91b4d267011f2d738b4fc3acb3f0f2377
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832699"
---
# <a name="the-progress-command"></a>命令的進度



執行命令時，會顯示進度。 您可以使用 **/進度**與任何其他您執行 WDSUTIL 命令。 請注意，您必須指定 **/verbose**並 **/進度**緊接著**WDSUTIL**。

## <a name="syntax"></a>語法

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>範例

若要初始化伺服器，並顯示進度，請輸入：
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:"C:\RemoteInstall"
```