---
title: bitsadmin sethelpertoken
description: 適用於 Windows 命令主題**bitsadmin sethelpertoken** -設定為 BITS 傳送工作的協助程式語彙基元的目前命令提示字元的主要權杖 （或任意的本機使用者帳戶的語彙基元，如果指定）。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 558a1aca66a7b3ec447136ceff9237d13efe4ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852999"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

設定目前命令提示字元的主要權杖 （或任意的本機使用者帳戶的語彙基元，如果指定） 與 BITS 傳送工作 [協助程式的語彙基元](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)。

**3.0 版和更早版本的位元**: 未支援。

## <a name="syntax"></a>語法

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID。|
|\<username@domain\> \<password\>|選擇性&mdash;帳戶使用權杖的本機使用者的認證。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
