---
title: Reg delete
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 369ef3bda37ab8e143a14f0f9707b9bbf14bd5f8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877079"
---
# <a name="reg-delete"></a>Reg delete



從登錄刪除一個子機碼或項目。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName>|指定要刪除的項目之子機碼的完整路徑。 若要指定遠端電腦，包含電腦名稱 (格式\\ \\ComputerName\)一部分*KeyName*。 省略\\ \\ComputerName\ 會導致作業到本機電腦的預設值。 *KeyName*必須包含有效的根機碼。 在本機電腦的有效的根機碼如下：HKLM、 HKCU、 HKCR、 HKU 和 HKCC。 如果指定的遠端電腦，則會是有效的根機碼：HKLM 和 HKU。|
|/v \<ValueName>|刪除子機碼下的特定項目。 如果未不指定任何項目，然後將刪除所有項目和子機碼下的子機碼。|
|/ve|指定只有沒有值的項目將被刪除。|
|/va|刪除指定的子機碼下的所有項目。 不會刪除指定的子機碼下的子機碼。|
|/f|刪除現有的登錄子機碼或項目，而不要求確認。|
|/?|顯示的說明**reg 刪除**在命令提示字元。|

## <a name="remarks"></a>備註

下表列出的傳回值**reg 刪除**作業。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="BKMK_examples"></a>範例

若要刪除的登錄機碼的逾時和其所有子機碼和值，請輸入：
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
若要刪除的登錄值 MTU HKLM\Software\MyCo 底下名為 ZODIAC 電腦上，輸入：
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)