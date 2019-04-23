---
title: bitsadmin replaceremoteprefix
description: 適用於 Windows 命令主題**bitsadmin replaceremoteprefix** -其遠端的 URL 開頭為作業中所有的檔案*OldPrefix*變更為使用*NewPrefix*。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3d3eba4f62842fa7f862cd4eaea6830e6a08397a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868129"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix



中的所有檔案作業的遠端 URL 開頭*OldPrefix*變更為使用*NewPrefix*。

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

## <a name="BKMK_examples"></a>範例

下列範例會變更名為作業中的所有檔案*myDownloadJob*其遠端的 URL 開頭*http://stageserver*來*http://prodserver*。
```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>其他資訊

[命令列語法關鍵](command-line-syntax-key.md)