---
title: 詳細資訊的命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a655ccdbd95b2f3523babecaa713ccdf99f9ec7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827239"
---
# <a name="the-verbose-command"></a>詳細資訊的命令



顯示指定命令的詳細資訊輸出。 您可以使用 **/verbose**與任何其他您執行 WDSUTIL 命令。 請注意，您必須指定 **/verbose**並 **/進度**緊接著**WDSUTIL**。

## <a name="syntax"></a>語法

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>範例

若要從自動新增資料庫刪除已核准的電腦，並顯示詳細資訊輸出，請輸入：
```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```