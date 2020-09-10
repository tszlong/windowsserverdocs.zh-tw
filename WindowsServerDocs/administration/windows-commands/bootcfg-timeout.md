---
title: bootcfg timeout
description: 適用于 bootcfg timeout 命令的參考文章，此命令會變更作業系統超時值。
ms.topic: reference
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 520889fce38eb48fe56b9b4c0a38277b5c7f8c8c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630111"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更作業系統超時值。

## <a name="syntax"></a>語法

```
bootcfg /timeout <timeoutvalue> [/s <computer> [/u <domain>\<user> /p <password>]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `/timeout <timeoutvalue>` | 在 [開機載入器] 區段中指定超時值。 `<timeoutvalue>`是在 NTLDR 載入預設值之前，使用者必須從開機載入器畫面選取作業系統的秒數。 的有效範圍 `<timeoutvalue>` 是0-999。 如果值為0，NTLDR 會立即啟動預設的作業系統，而不會顯示開機載入器畫面。 |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 使用或所指定之使用者的帳戶許可權來執行命令 `<user>` `<domain>\<user>` 。 預設值是發出命令的電腦上目前登入之使用者的許可權。 |
| `/p <password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要使用 **bootcfg/timeout** 命令：

```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)
