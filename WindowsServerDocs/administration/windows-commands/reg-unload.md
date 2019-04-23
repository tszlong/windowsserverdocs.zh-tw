---
title: Reg 卸載
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aaa7d7a9fa82db2968d988e3b7b3fb8275a72337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834979"
---
# <a name="reg-unload"></a>Reg 卸載



移除登錄已載入使用之區段**reg 負載**作業。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
reg unload <KeyName>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<KeyName>|指定要卸載的子機碼的完整路徑。 指定遠端電腦，包含電腦名稱 (格式\\ \\ComputerName\)一部分*KeyName*。 省略\\ \\ComputerName\ 會導致作業到本機電腦的預設值。 *KeyName*必須包含有效的根機碼。 有效的根機碼的本機電腦的 HKLM、 HKCU、 HKCR、 HKU 和 HKCC。 如果指定的遠端電腦，則有效的根機碼的 HKLM 和 HKU。|
|/?|顯示的說明**reg 卸載**在命令提示字元。|

## <a name="remarks"></a>備註

下表列出的傳回值**reg 卸載**選項。

|值|描述|
|-----|-----------|
|0|成功|
|1|失敗|

## <a name="BKMK_examples"></a>範例

若要解除載入 hive TempHive HKLM 檔案中，輸入：
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> 除非您別無選擇，否則請勿直接編輯登錄。 登錄編輯程式略過標準保護，讓設定可以使效能降低、 損害您的系統，或甚至要求您必須重新安裝 Windows。 您可以安全地改變大部分的登錄設定，使用 控制台 或 Microsoft Management Console (MMC) 中的程式。 如果您必須直接編輯登錄，請將它備份第一個。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)