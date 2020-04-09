---
title: reg delete
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 726a3c700a9278dbc7abb1873aae7ea3c957bbb5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836501"
---
# <a name="reg-delete"></a>reg delete



從登錄中刪除子機碼或專案。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName >|指定要刪除之子機碼或專案的完整路徑。 若要指定遠端電腦，請包含電腦名稱稱（格式為 \\\\ComputerName\) 作為*KeyName*的一部分。 省略 \\\\ComputerName \ 會使操作預設為本機電腦。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰為： HKLM 和 HKU。|
|/v \<ValueName >|刪除子機碼下的特定專案。 如果未指定任何專案，則會刪除子機碼下的所有專案和子機碼。|
|/ve|指定只會刪除沒有值的專案。|
|/va|刪除指定子機碼下的所有專案。 不會刪除指定子機碼下的子機碼。|
|/f|刪除現有的登錄子機碼或專案，而不要求確認。|
|/?|在命令提示字元中顯示**reg delete**的說明。|

## <a name="remarks"></a>備註

下表列出**reg delete**作業的傳回值。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要刪除登錄機碼的 Timeout 和其所有子機碼和值，請輸入：
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
若要在名為天干//的電腦上刪除 HKLM\Software\MyCo 下的登錄值 MTU，請輸入：
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)