---
title: prompt
description: 了解如何自訂您的命令提示字元。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5ef487ce9799c1f09660cdfcd6fba71336fc4d9a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442144"
---
# <a name="prompt"></a>prompt



變更 Cmd.exe 命令提示字元。 如果未指定參數，使用**提示字元**重設為預設值，這是目前的磁碟機代號，後面接著大於符號的目錄的命令提示字元 ( **>** )。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
prompt [<Text>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Text>|指定的文字和您想要在命令提示字元中包含的資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

您可以自訂命令提示字元中，以顯示您要包括目前的目錄、 時間和日期，以及 Microsoft Windows 版本號碼這類資訊做為名稱的任何文字。

下表列出，而不是，或是加上一個或多個字元字串中，您可以包含的字元組合*文字*參數。 此清單包括每個字元的組合將加入至命令提示字元中的資訊之文字的簡短描述。  

| 字元 |                                 描述                                 |
|-----------|-----------------------------------------------------------------------------|
|    $q     |                               = （等號）                                |
|    $$     |                               $ （貨幣符號）                               |
|    $t     |                                目前的時間                                 |
|    $d     |                                目前的日期                                 |
|    $p     |                           目前的磁碟機和路徑                            |
|    $v     |                           Windows 版本號碼                            |
|    $n     |                                目前的磁碟機                                |
|    $g     |                            > （大於符號）                            |
|    $l     |                             < （小於登）                              |
|    $b     |                                                                             |
|    $_     |                               輸入-換行字元                                |
|    $e     |                         ANSI 逸出代碼 （27）                          |
|    $h     | 退格鍵 （若要刪除的命令列已寫入的字元） |
|    $     |                                & （連字號）                                |
|    $c     |                            （（左括號）                             |
|    $f     |                            ) （右括號）                            |
|    $s     |                                    空間                                    |

命令延伸模組啟用時 （也就是預設值）**提示字元**命令支援下列格式的字元：  

|字元|描述|
|---------|-----------|
|$+|零個以上的加號 ( **+** ) 個字元，根據的深度**pushd**目錄堆疊 （針對每個層級推送的一個字元）。|
|$m|遠端的名稱，如果目前的磁碟機不是網路磁碟機相關聯的目前磁碟機代號或空字串。|

如果您納入 **$p**字元輸入 （用來判斷目前的磁碟機和路徑） 的每個命令之後，您的磁碟讀取文字參數。 這可能需要額外的時間，特別是針對軟碟機。

## <a name="BKMK_examples"></a>範例

若要設定的目前時間和日期的兩行命令提示字元的第一行和大於符號在下一行中，輸入：
```
prompt $d$s$s$t$_$g 
```
在提示字元，如下所示，其中的日期和時間是最新變更：
```
Fri 06/01/2007  13:53:28.91
>
```
若要設定 命令提示字元中顯示為箭號 (`-->`)，型別：
```
prompt --$g
```
若要手動變更預設設定 （目前的磁碟機和路徑，後面接著大於符號） 的命令提示字元中，輸入：
```
prompt $p$g
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)