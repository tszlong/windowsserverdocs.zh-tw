---
title: bitsadmin sethelpertoken
description: 適用于 bitsadmin sethelpertoken 的 Windows 命令主題，它會將目前的命令提示字元主要權杖（或任意本機使用者帳戶的權杖，如果有指定）設定為 BITS 傳送作業的 helper token。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a1e8fd0054cadf3bf06b6e5b7bdf5010b18781e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849531"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

將目前的命令提示字元主要權杖（或任意本機使用者帳戶的權杖，如果有指定）設定為 BITS 傳送作業的 [helper token](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)。

**BITS 3.0 和更早版本**：不支援。

## <a name="syntax"></a>語法

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID。|
|\<username@domain\> \<密碼\>|選擇性&mdash;要使用其權杖的本機使用者帳號憑證。|

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
