---
title: bitsadmin getnotifyinterface
description: 適用于**bitsadmin getnotifyinterface**的 Windows 命令主題，可判斷另一個程式是否已為指定的作業註冊 COM 回呼介面。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5eb5aee42446c70f16fd6785a3645f42c1987e4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850571"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

判斷另一個程式是否已為指定的作業註冊 COM 回呼介面（通知介面）。

## <a name="syntax"></a>語法

```
bitsadmin /getnotifyinterface <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |

#### <a name="output"></a>輸出

此命令的輸出會顯示、**已註冊或已**取消**註冊**。

> [!NOTE]
> 您無法判斷註冊回呼介面的程式。

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會針對名為*myDownloadJob*的作業，抓取通知介面。

```
C:\>bitsadmin /getnotifyinterface myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)