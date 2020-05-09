---
title: dcgpofix
description: Dcgpofix 命令的參考主題，會重新建立網域的預設群組原則物件（Gpo）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f11b7db8110cd2d7dcf08cd250eba411e7ff21a8
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993172"
---
# <a name="dcgpofix"></a>dcgpofix

重新建立網域的預設群組原則物件（Gpo）。 若要取得群組原則管理主控台（GPMC），您必須透過伺服器管理員將群組原則管理安裝為功能。

>[!IMPORTANT]
> 最佳做法是只設定預設網域原則 GPO 來管理預設的**帳戶原則**設定、密碼原則、帳戶鎖定原則和 Kerberos 原則。 此外，您應該只設定 [預設網域控制站原則] GPO 來設定使用者權限和稽核原則。

## <a name="syntax"></a>語法

```
dcgpofix [/ignoreschema] [/target: {domain | dc | both}] [/?]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /ignoreschema | 當您執行此命令時，會忽略 Active Directory 架構的版本。 否則，此命令僅適用于隨附命令之 Windows 版本的相同架構版本。 |
| `/target {domain | dc | both` | 指定是以預設網域原則、預設網域控制站原則或兩種原則類型為目標。 |
| /? | 在命令提示字元顯示 [說明]。 |

## <a name="examples"></a>範例

若要管理預設的**帳戶原則**設定、密碼原則、帳戶鎖定原則和 Kerberos 原則，而忽略 Active Directory 架構版本，請輸入：

```
dcgpofix /ignoreschema /target:domain
```

若要只設定 [預設網域控制站原則] GPO 來設定使用者權限和稽核原則，同時忽略 Active Directory 架構版本，請輸入：

```
dcgpofix /ignoreschema /target:dc
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)