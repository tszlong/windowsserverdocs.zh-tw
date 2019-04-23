---
title: makecab
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e353f2f97ef55e806d991b45018755847fca0364
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871399"
---
# <a name="makecab"></a>makecab

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

現有的檔案封裝的封包 (.cab) 檔案。
## <a name="syntax"></a>語法
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<source>|若要壓縮的檔案。|
|<destination>|提供給壓縮的檔案的檔案名稱。 如果省略，則原始程式檔名稱的最後一個字元是以底線 (_) 取代，並做為目的地。|
|/f <directives_file>|副檔名**makecab**指示詞 （可能會重複）。|
|/d var=<value>|定義具有指定值的變數。|
|/l <dir>|將目的地位置 （預設為目前的目錄）。|
|/v[<n>]|設定偵錯的詳細資訊層級 (0 = none，...，3 = full)。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
-   請參閱[Microsoft 封包格式](https://go.microsoft.com/fwlink/?LinkId=226852)directive_file 有關 MSDN 上。

## <a name="additional-references"></a>其他參考資料
-   [命令列語法關鍵](command-line-syntax-key.md)

