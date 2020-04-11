---
title: bitsadmin geterrorcount
description: 適用于**bitsadmin geterrorcount**的 Windows 命令主題，它會抓取指定之作業產生暫時性錯誤的次數計數。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef0bf043517d4edfa8d72888746ca5d9c92ecc21
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123130"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount

抓取指定的作業產生暫時性錯誤的次數計數。

## <a name="syntax"></a>語法

```
bitsadmin /geterrorcount <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會抓取名為*myDownloadJob*之作業的錯誤計數資訊。

```
C:\>bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)