---
title: 使用 verbose 命令
description: 詳細資訊的參考文章，顯示指定命令的詳細資訊輸出。
ms.topic: reference
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a5c05590bbbb3f1b185a64d6b0081a3d230d6b41
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640454"
---
# <a name="using-the-verbose-command"></a>使用 verbose 命令

顯示指定命令的詳細資訊輸出。 您可以將 **/verbose** 與您執行的任何其他 WDSUTIL 命令搭配使用。 請注意，您必須在**WDSUTIL**之後直接指定 **/verbose**和 **/progress** 。

## <a name="syntax"></a>語法

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>範例

若要從自動新增資料庫中刪除核准的電腦並顯示詳細資訊輸出，請輸入：

```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```