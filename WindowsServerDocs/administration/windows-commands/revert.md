---
title: revert
description: '* * * * 的參考文章'
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 719d1be30fd1d2874231e50b5cecb1604712c8b2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883614"
---
# <a name="revert"></a>revert



將磁片區還原回指定的陰影複製。 只有 CLIENTACCESSIBLE 內容中的陰影複製才支援這項功能。 這些陰影複製是持續性的，而且只能由系統提供者進行。 如果使用時不含參數， **revert**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
revert <ShadowCopyID>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<ShadowCopyID>|指定要復原磁碟區的陰影複製識別碼。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)