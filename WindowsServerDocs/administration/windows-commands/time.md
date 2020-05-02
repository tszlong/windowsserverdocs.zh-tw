---
title: time
description: 瞭解如何設定和顯示系統時間。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e27b260bdaa8896ad3cf0ad58294467bbb63e1c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721357"
---
# <a name="time"></a>time



顯示或設定系統時間。 如果在沒有參數的情況下使用， **time**會顯示目前的系統時間，並提示您輸入新的時間。



## <a name="syntax"></a>語法

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<HH> [：\<MM> [：\<SS> [。\<NN>]]] [am\|pm]|將系統時間設定為指定的新時間，其中*HH*是以小時為單位， *MM*為分鐘，而*SS*則以秒為單位。 *NN*可以用來指定百分之一秒。 如果未指定**am**或**pm** ，**時間**預設會使用24小時制格式。|
|/t|顯示目前的時間，而不提示您輸入新的時間。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要變更目前的時間，您必須具有系統管理認證。
-   您必須以冒號（:) 分隔*HH*、 *MM*和*SS*的值。 *SS*和*NN*必須以句點（.）分隔。
-   有效的*HH*值為0到24。
-   有效的*MM*和*SS*值為0到59。

## <a name="examples"></a><a name="BKMK_examples"></a>範例

如果已啟用命令延伸模組，若要顯示目前的系統時間，請輸入：
```
time /t
```
若要將目前的系統時間變更為下午5:30，請輸入下列其中一項：
```
time 17:30:00
time 5:30 pm
```
若要顯示目前的系統時間，接著輸入新時間的提示，請輸入：
```
The current time is: 17:33:31.35
Enter the new time:
```
若要保留目前的時間並返回命令提示字元，請按 ENTER。 若要變更目前的時間，請輸入新的時間，然後按 ENTER 鍵。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)