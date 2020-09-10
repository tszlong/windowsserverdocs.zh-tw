---
title: telnet unset
description: Telnet 取消設定的參考文章，這會關閉先前設定的選項。
ms.topic: reference
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 96b8126d758d5277129f88f193cfea4b25e80584
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640817"
---
# <a name="telnet-unset"></a>telnet： unset

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

關閉先前設定的選項。

## <a name="syntax"></a>語法
```
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]
```
#### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|bsasdel|將**倒退鍵**傳送**倒退鍵。**|
|crlf|以 CR 的形式傳送 **Enter** 鍵。 也稱為換行模式。|
|delasbs|將 **delete** 傳送為 **delete**。|
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
