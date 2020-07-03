---
title: verbose
description: 詳細資訊的參考文章，會顯示指定命令的詳細資訊輸出。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 079e441ba4a932e23493e7e37fbe36cab4c4971f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935239"
---
# <a name="verbose"></a>verbose

顯示指定命令的詳細資訊輸出。 您可以將 **/verbose**與您執行的任何其他 WDSUTIL 命令搭配使用。 請注意，您必須在**WDSUTIL**之後直接指定 **/verbose**和 **/progress** 。

## <a name="syntax"></a>語法

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>範例

若要從自動新增資料庫中刪除已核准的電腦，並顯示詳細資訊輸出，請輸入：
```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```