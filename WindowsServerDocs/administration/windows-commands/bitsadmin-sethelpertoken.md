---
title: bitsadmin sethelpertoken
description: 適用于**bitsadmin sethelpertoken**的 Windows 命令主題-將目前的命令提示字元主要權杖（或任意本機使用者帳戶的權杖，如果有指定）設定為 BITS 傳送作業的 helper token。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 91c03366998168dad9ab4530ef36a5020b8ad6ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380574"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

將目前的命令提示字元主要權杖（或任意本機使用者帳戶的權杖，如果有指定）設定為 BITS 傳送作業的 [helper token](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)。

**BITS 3.0 和更早版本**：不支援。

## <a name="syntax"></a>語法

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID。|
|\<username@domain\> \<密碼\>|選擇性&mdash;要使用其權杖的本機使用者帳號憑證。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
