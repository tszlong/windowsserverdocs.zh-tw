---
title: time
description: 了解如何設定及顯示的系統時間。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5c1f43be98a19c4b150c247cc7fd48d62edeb5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861909"
---
# <a name="time"></a>time



顯示或設定系統時間。 如果未指定參數，使用**時間**會顯示目前的系統時間，並會提示您輸入新的時間。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<HH > [:\<公釐 > [:\<SS > [。\<NN >]]] [我\|pm]|設定系統時間指定，其中的新時間*HH*是以小時為單位 （必要）， *MM*以分鐘為單位，並*SS*以秒為單位。 *NN*可用來指定的百分之一秒。 如果**我**或是**pm**未指定，則**時間**預設會使用 24 小時制。|
|/t|顯示目前的時間，而不提示您輸入新的時間。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要變更目前的時間，您必須具有系統管理認證。
-   您必須分隔的值*HH*， *MM*，並*SS*加上冒號 （:）。 *SS*並*NN*都必須以句號 （.） 分隔。
-   有效*HH*值為 0 到 24。
-   有效*公釐*並*SS*的值為 0 到 59。

## <a name="BKMK_examples"></a>範例

如果已啟用命令延伸模組，以顯示目前的系統時間中，輸入：
```
time /t
```
若要變更目前的系統時間到下午 5:30，輸入下列其中一項：
```
time 17:30:00
time 5:30 pm
```
若要顯示目前的系統時間，後面接著提示字元中輸入新的時間，請輸入：
```
The current time is: 17:33:31.35
Enter the new time:
```
若要保留目前的時間，並返回命令提示字元，按 ENTER 鍵。 若要變更目前的時間，輸入新的時間，然後按 ENTER。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)