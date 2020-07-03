---
title: telnet unset
description: 解除登錄 telnet 的參考文章，這會關閉先前設定的選項。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3efbf7d4f2507d16dbe0beb704ab0c80467a56d3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935293"
---
# <a name="telnet-unset"></a>telnet：未設定

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

關閉先前設定的選項。

## <a name="syntax"></a>語法
```
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]
```
#### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|bsasdel|將**倒退鍵**傳送為**退**格鍵。|
|crlf|以 CR 的形式傳送**Enter**鍵。 也稱為換行模式。|
|delasbs|傳送**刪除**做為**刪除**。|
|逸出|移除 escape 字元設定。|
|localecho|關閉 localecho。|
|logging|關閉記錄。|
|ntlm|關閉 NTLM 驗證。|
|?|顯示此命令的說明。|
## <a name="examples"></a>範例
關閉記錄功能。
```
u logging
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
