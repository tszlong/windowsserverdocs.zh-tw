---
title: 使用進度命令
description: 進度的參考文章，會在命令執行時顯示進度。
ms.topic: reference
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 51f127cd9d707652e43a5cae2a2e0bfd7a573b69
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640787"
---
# <a name="using-the-progress-command"></a>使用進度命令

顯示命令執行時的進度。 您可以使用 **/progress** 搭配您所執行的任何其他 WDSUTIL 命令。 請注意，您必須在**WDSUTIL**之後直接指定 **/verbose**和 **/progress** 。

## <a name="syntax"></a>語法

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>範例

若要初始化伺服器並顯示進度，請輸入：
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:C:\RemoteInstall
```