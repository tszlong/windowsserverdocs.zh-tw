---
title: revert
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bc77b17317f602d642c7a9e025b67be10ad7256
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875109"
---
# <a name="revert"></a>revert



還原至指定的陰影複製磁碟區。 這是僅支援 CLIENTACCESSIBLE 內容中的陰影複製。 這些陰影複製是持續性，並只能由系統提供者。 如果未指定參數，使用**還原**在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
revert <ShadowCopyID>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ShadowCopyID>|指定要還原的磁碟區的陰影複製識別碼。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)