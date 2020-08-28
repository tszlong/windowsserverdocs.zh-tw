---
title: lpq
description: Lpq 命令的參考文章，此命令會顯示執行線上印表機 (LPD) 之電腦上的列印佇列狀態。
ms.topic: reference
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d495923b94884f0d4538839fcd3c1193e73ee938
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023702"
---
# <a name="lpq"></a>lpq

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示執行線上印表機 Daemon (LPD) 之電腦上的列印佇列狀態。

## <a name="syntax"></a>語法

```
lpq -S <servername> -P <printername> [-l]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -S `<servername>` | 依名稱或 IP 位址指定 (，) 以您要顯示的狀態裝載 LPD 列印佇列的電腦或印表機共用裝置。 此為必要參數，且必須為大寫。 |
| -P `<Printername>` | 依名稱指定 (，) 具有您要顯示之狀態的列印佇列印表機。 此為必要參數，且必須為大寫。 |
| -l | 指定您想要顯示有關列印佇列狀態的詳細資料。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要在*10.0.0.45*的 LPD 主機上顯示*Laserprinter1*印表機佇列的狀態，請輸入：

```
lpq -S 10.0.0.45 -P Laserprinter1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)
