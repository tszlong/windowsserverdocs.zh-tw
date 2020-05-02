---
title: reg delete
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4ff643970bac021a6b7dcb731e64c412deb8df3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722569"
---
# <a name="reg-delete"></a>reg delete



從登錄中刪除子機碼或專案。



## <a name="syntax"></a>語法

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName>|指定要刪除之子機碼或專案的完整路徑。 若要指定遠端電腦，請將電腦名稱稱\\ \\（格式為\) *ComputerName 的一部分*）包含在內。 省略\\ \\ComputerName \ 會使此操作預設為本機電腦。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰為： HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰為： HKLM 和 HKU。|
|/v \<ValueName>|刪除子機碼下的特定專案。 如果未指定任何專案，則會刪除子機碼下的所有專案和子機碼。|
|/ve|指定只會刪除沒有值的專案。|
|/va|刪除指定子機碼下的所有專案。 不會刪除指定子機碼下的子機碼。|
|/f|刪除現有的登錄子機碼或專案，而不要求確認。|
|/?|在命令提示字元中顯示**reg delete**的說明。|

## <a name="remarks"></a>備註

下表列出**reg delete**作業的傳回值。

|值|描述|
|-----|-----------|
|0|Success|
|1|失敗|

## <a name="examples"></a>範例

若要刪除登錄機碼的 Timeout 和其所有子機碼和值，請輸入：
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
若要在名為天干//的電腦上刪除 HKLM\Software\MyCo 下的登錄值 MTU，請輸入：
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)