---
title: tzutil
description: Tzutil 的參考文章，它會顯示 Windows 時區公用程式。
ms.topic: reference
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4ed266c0a8f8b8e45c6da76958a770dec5328db6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626602"
---
# <a name="tzutil"></a>tzutil

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示 Windows 時區公用程式。

## <a name="syntax"></a>語法
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/?|在命令提示字元顯示說明。|
|/g|顯示目前的時區識別碼。|
|/s \<timeZoneID> [_dstoff]|使用指定的時區識別碼，設定目前的時區。 **_Dstoff**尾碼會針對適用) 的時區 (停用日光節約時間調整。|
|/l|列出所有有效的時區識別碼和顯示名稱。 輸出將是：<p>-   \<display name><br />-   \<time zone ID>|

## <a name="remarks"></a>備註
結束代碼為 **0** 表示已順利完成命令。

## <a name="examples"></a>範例
若要顯示目前的時區識別碼，請輸入：
```
tzutil /g
```
若要將目前的時區設定為太平洋標準時間，請輸入：
```
tzutil /s Pacific Standard time
```
若要將目前的時區設定為太平洋標準時間，並停用日光節約時間調整，請輸入：
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

