---
title: 使用 verbose 命令
description: 詳細資訊的參考文章，顯示指定命令的詳細資訊輸出。
ms.topic: reference
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2335ef8bf3e3b231851d424f99f0a7e878218c4b
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524873"
---
# <a name="using-the-verbose-command"></a>使用 verbose 命令

顯示指定命令的詳細資訊輸出。 您可以將 **/verbose** 與您執行的任何其他 wdsutil 命令搭配使用。 請注意，您必須在**wdsutil**之後直接指定 **/verbose**和 **/progress** 。

## <a name="syntax"></a>語法

```
wdsutil /verbose <commands>
```

## <a name="examples"></a>範例

若要從自動新增資料庫中刪除核准的電腦並顯示詳細資訊輸出，請輸入：

```
wdsutil /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```