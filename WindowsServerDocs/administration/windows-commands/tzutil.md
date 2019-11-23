---
title: tzutil
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 347254bd5a00a8bfb4a80f20d518f1e0e8b593bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392309"
---
# <a name="tzutil"></a>tzutil

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示 Windows 時區公用程式。 
## <a name="syntax"></a>語法
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
### <a name="parameters"></a>Parameters
|參數|描述|
|-------|--------|
|/?|在命令提示字元顯示說明。|
|/g|顯示目前的時區識別碼。|
|/s \<timeZoneID > [_dstoff]|使用指定的時區識別碼來設定目前的時區。 **_Dstoff**尾碼會停用時區的日光節約時間調整（如果適用）。|
|/l|列出所有有效的時區識別碼和顯示名稱。 輸出將會是：<br /><br />-   \<顯示名稱 ><br />-   \<時區識別碼 >|

## <a name="remarks"></a>備註
結束代碼為**0**表示命令已順利完成。

## <a name="BKMK_Examples"></a>典型
若要顯示目前的時區識別碼，請輸入：
```
tzutil /g
```
若要將目前的時區設定為太平洋標準時間，請輸入：
```
tzutil /s Pacific Standard time
```
若要將目前的時區設定為太平洋標準時間並停用日光節約時間調整，請輸入：
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>其他參考
-   [命令列語法關鍵](command-line-syntax-key.md)

