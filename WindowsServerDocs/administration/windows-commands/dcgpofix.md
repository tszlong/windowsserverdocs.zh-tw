---
title: dcgpofix
description: Dcgpofix 命令的參考文章，會重新建立網域 (Gpo) 的預設群組原則物件。
ms.topic: reference
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3ee90daa4e0bc6100c28f06c892694e015976f8a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628950"
---
# <a name="dcgpofix"></a>dcgpofix

重新建立網域 (Gpo) 的預設群組原則物件。 若要取得 (GPMC) 的群組原則管理主控台，您必須透過伺服器管理員以功能的形式安裝群組原則管理。

>[!IMPORTANT]
> 最佳做法是，您應該只將預設網域原則 GPO 設定為管理預設的 **帳戶原則** 設定、密碼原則、帳戶鎖定原則和 Kerberos 原則。 此外，您應該只設定預設網域控制站原則 GPO 來設定使用者權限和稽核原則。

## <a name="syntax"></a>語法

```
dcgpofix [/ignoreschema] [/target: {domain | dc | both}] [/?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /ignoreschema | 當您執行此命令時，會忽略 Active Directory 架構的版本。 否則，此命令只適用于與隨附命令的 Windows 版本相同的架構版本。 |
| `/target {domain | dc | both` | 指定是否以預設網域原則、預設網域控制站原則或這兩種類型的原則為目標。 |
| /? | 在命令提示字元顯示 [說明]。 |

## <a name="examples"></a>範例

若要管理預設的 **帳戶原則** 設定、密碼原則、帳戶鎖定原則和 Kerberos 原則，並忽略 Active Directory 架構版本，請輸入：

```
dcgpofix /ignoreschema /target:domain
```

若要將預設的網域控制站原則 GPO 設定為僅設定使用者權限和稽核原則，同時忽略 Active Directory 架構版本，請輸入：

```
dcgpofix /ignoreschema /target:dc
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)