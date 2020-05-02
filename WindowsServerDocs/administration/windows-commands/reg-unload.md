---
title: reg unload
description: Reg unload 命令的參考主題，其會移除使用 reg 載入作業載入的登錄區段。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 029b9225f8a437be18c3056d97e153075d9df7c9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722496"
---
# <a name="reg-unload"></a>reg unload



移除使用**reg 載入**作業載入的登錄區段。



## <a name="syntax"></a>語法

```
reg unload <KeyName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName>|指定要卸載之子機碼的完整路徑。 若要指定遠端電腦，請包含電腦名稱稱\\ \\（格式為 ComputerName\) *的一部分）。* 省略\\ \\ComputerName \ 會使此操作預設為本機電腦。 *KeyName*必須包含有效的根金鑰。 本機電腦的有效根金鑰為 HKLM、HKCU、HKCR、HKU 和 HKCC。 如果指定遠端電腦，有效的根金鑰就是 HKLM 和 HKU。|
|/?|在命令提示字元中顯示**reg unload**的說明。|

## <a name="remarks"></a>備註

下表列出**reg unload**選項的傳回值。

|值|描述|
|-----|-----------|
|0|Success|
|1|失敗|

## <a name="examples"></a>範例

若要卸載 file HKLM 中的 hive TempHive，請輸入：
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> 請勿直接編輯登錄，除非您沒有替代方案。 登錄編輯程式會略過標準保護，允許降低效能、損毀您的系統，甚至要求您重新安裝 Windows 的設定。 您可以使用 [控制台] 或 Microsoft Management Console （MMC）中的程式，安全地改變大部分的登錄設定。 如果您必須直接編輯登錄，請先備份。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [reg load 命令](reg-load.md)