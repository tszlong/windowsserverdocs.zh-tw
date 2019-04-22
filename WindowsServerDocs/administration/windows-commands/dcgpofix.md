---
title: dcgpofix
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91fceb429ca00b1b3d9d36d01f5e97cfd464ccb9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825159"
---
# <a name="dcgpofix"></a>dcgpofix



重新建立網域預設群組原則物件 (Gpo)。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/ignoreschema|會忽略 Active Directory® 結構描述 mc 版</br>當您執行此命令。 否則，命令只能用於命令送交的 Windows 版本相同的結構描述版本。|
|/target {網域 | DC | 兩者}|指定要還原哪一個 GPO。 您可以還原預設網域原則 GPO、 預設網域控制站 GPO 中，或兩者。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

-   **Dcgpofix**命令適用於 Windows Server 2008 R2 和 Windows Server 2008，除非在 Server Core 安裝上。
-   雖然群組原則管理主控台 (GPMC) 隨附於 Windows Server 2008 R2 和 Windows Server 2008，您必須安裝群組原則管理，為透過 伺服器管理員的功能。

## <a name="BKMK_Examples"></a>範例

將 預設網域原則 GPO 還原為其原始狀態。 您將會遺失任何您對此 GPO 的變更。 最佳做法，您應該設定您管理的預設帳戶原則設定、 密碼原則、 帳戶鎖定原則，以及 Kerberos 原則 預設網域原則 GPO。 在此範例中，您可以忽略 Active Directory 結構描述版本，讓**dcgpofix**命令並不限於相同的結構描述命令送交的 Windows 版本。
```
dcgpofix /ignoreschema /target:Domain
```
將 預設網域控制站原則 GPO 還原為其原始狀態。 您將會遺失任何您對此 GPO 的變更。 最佳做法，您應該設定 預設網域控制站原則 GPO 來設定使用者權限和稽核原則。 在此範例中，您可以忽略 Active Directory 結構描述版本，讓**dcgpofix**命令並不限於相同的結構描述命令送交的 Windows 版本。
```
dcgpofix /ignoreschema /target:DC
```

#### <a name="additional-references"></a>其他參考資料

-   [群組原則 TechCenter](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [命令列語法關鍵](command-line-syntax-key.md)