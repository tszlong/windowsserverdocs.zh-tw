---
title: bootcfg timeout
description: 適用於 Windows 命令主題**bootcfg 逾時**-變更作業系統逾時值。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fc33d2d20d6d2532c5ed1f33e27a768935d1e85
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434647"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更作業系統逾時值。

## <a name="syntax"></a>語法
```
bootcfg /timeout <timeOutValue> [/s <computer> [/u <Domain\User>/p <Password>]]
```
## <a name="parameters"></a>參數

|        參數        |                                                                                                                                                                                  描述                                                                                                                                                                                   |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /timeout <timeOutValue> | 在 [開機載入器] 區段中指定的逾時值。 <timeOutValue>使用者可以從開機載入器畫面選取的作業系統，NTLDR 會載入預設值之前的秒數。 有效範圍為<timeOutValue>0 999。 如果值為 0，則 NTLDR 立即啟動預設的作業系統而不會顯示開機載入器畫面。 |
|      /s <computer>      |                                                                                                                               指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。                                                                                                                               |
|    /u <Domain\User>     |                                                                                       使用指定的使用者帳戶權限執行命令<User>或 < 網域 \ 使用者 >。 預設值是目前登入的使用者發出命令的電腦上的權限。                                                                                        |
|      /p <Password>      |                                                                                                                                            指定<Password>中指定的使用者帳戶 **/u**參數。                                                                                                                                             |
|           /?            |                                                                                                                                                                      在命令提示字元顯示說明。                                                                                                                                                                      |

## <a name="BKMK_examples"></a>範例
下列範例示範如何使用**bootcfg /timeout**命令：
```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)
