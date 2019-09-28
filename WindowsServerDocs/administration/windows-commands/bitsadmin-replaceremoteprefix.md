---
title: bitsadmin replaceremoteprefix
description: 適用于**bitsadmin replaceremoteprefix**的 Windows 命令主題-遠端 URL 開頭為*OldPrefix*的作業中的所有檔案都會變更為使用*NewPrefix*。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee896a337b571487797967d3ce0bf1f1b17e7507
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380805"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

作業中其遠端 URL 開頭為*OldPrefix*的所有檔案都會變更為使用*NewPrefix*。

## <a name="syntax"></a>語法

```
bitsadmin /ReplaceRemotePrefix <Job> <OldPrefix> <NewPrefix
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|OldPrefix|現有的 URL 前置詞|
|NewPrefix|新的 URL 前置詞|

## <a name="examples"></a>範例

下列範例會變更名為*myDownloadJob*之作業中的所有檔案，其遠端 URL 的開頭為 *http://stageserver* 到 *http://prodserver* 。

```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>其他資訊

[命令列語法關鍵](command-line-syntax-key.md)