---
title: tzutil
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41a46ea7974b67cc557973484428480e7beb5484
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876799"
---
# <a name="tzutil"></a>tzutil

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示 Windows 時間區域公用程式。 
## <a name="syntax"></a>語法
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/?|在命令提示字元顯示說明。|
|/g|顯示目前的時區識別碼。|
|/s \<timeZoneID>[_dstoff]|設定目前的時區使用指定的時區識別碼。 **_Dstoff**後置詞在 （如果適用的話），會停用時區的日光節約時間調整。|
|/l|列出所有有效的時間區域識別碼，並顯示名稱。 輸出會是：<br /><br />-   \<顯示名稱 ><br />-   \<時區識別碼 >|

## <a name="remarks"></a>備註
結束代碼**0**指出命令已順利完成。

## <a name="BKMK_Examples"></a>範例
若要顯示目前的時區識別碼，請輸入：
```
tzutil /g
```
若要設定目前時區的時間為太平洋標準時間，請輸入：
```
tzutil /s Pacific Standard time
```
若要將目前的時區設定為太平洋標準時間，並停用日光節約時間調整，請輸入：
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)

