---
title: bitsadmin monitor
description: '**Bitsadmin monitor**的 Windows 命令主題，它會監視目前使用者所擁有的傳送佇列中的作業。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bda268afd5fda24bba2afb04b32bac9cda9a05bb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850211"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor

監視目前使用者所擁有之傳輸佇列中的作業。

## <a name="syntax"></a>語法

```
bitsadmin /monitor [/allusers] [/refresh <seconds>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| /allusers | 選擇性。 監視所有使用者的作業。 您必須具有系統管理員許可權，才能使用此參數。 |
| /refresh | 選擇性。 以 `<seconds>`所指定的間隔重新整理資料。 預設重新整理間隔為5秒。 若要停止重新整理，請按 CTRL + C。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會監視目前使用者所擁有之作業的傳送佇列，並每隔60秒重新整理一次資訊。

```
C:\>bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)