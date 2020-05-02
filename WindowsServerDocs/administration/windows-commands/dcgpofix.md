---
title: dcgpofix
description: Dcgpofix 的參考主題，會重新建立網域的預設群組原則物件（Gpo）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b30190f06e5e38031c8d205d8ccee9c573a8d84
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716782"
---
# <a name="dcgpofix"></a>dcgpofix

重新建立網域的預設群組原則物件（Gpo）。

## <a name="syntax"></a>語法

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

#### <a name="parameters"></a>參數

|    參數    |                                                                                                 描述                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /ignoreschema  | 忽略架構 mc®的 Active Directory 版本</br>當您執行此命令時。 否則，此命令僅適用于隨附命令之 Windows 版本的相同架構版本。 |
| /target {Domain |                                                                                                     DC                                                                                                      |
|       /?        |                                                                                    在命令提示字元顯示 [說明]。                                                                                     |

## <a name="remarks"></a>備註

-   **Dcgpofix**命令可在 windows Server 2008 R2 和 windows server 2008 中使用，但 Server Core 安裝除外。
-   雖然群組原則管理主控台（GPMC）是與 Windows Server 2008 R2 和 Windows Server 2008 一起散發，但您必須透過伺服器管理員將群組原則管理安裝為功能。

## <a name="examples"></a>範例

將預設網域原則 GPO 還原到其原始狀態。 您將會遺失對此 GPO 所做的任何變更。 最佳做法是只設定預設網域原則 GPO 來管理預設的帳戶原則設定、密碼原則、帳戶鎖定原則和 Kerberos 原則。 在此範例中，您會忽略 Active Directory 架構的版本，讓**dcgpofix**命令不限於與隨附命令的 Windows 版本相同的架構。
```
dcgpofix /ignoreschema /target:Domain
```
將預設網域控制站原則 GPO 還原為其原始狀態。 您將會遺失對此 GPO 所做的任何變更。 最佳做法是，只設定 [預設網域控制站原則] GPO 來設定使用者權限和稽核原則。 在此範例中，您會忽略 Active Directory 架構的版本，讓**dcgpofix**命令不限於與隨附命令的 Windows 版本相同的架構。
```
dcgpofix /ignoreschema /target:DC
```

## <a name="additional-references"></a>其他參考

-   [群組原則 TechCenter](https://go.microsoft.com/fwlink/?LinkID=145531)
-   - [命令列語法關鍵](command-line-syntax-key.md)