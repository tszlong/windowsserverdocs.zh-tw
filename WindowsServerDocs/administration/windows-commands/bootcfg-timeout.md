---
title: bootcfg timeout
description: 適用于 bootcfg timeout 的 Windows 命令主題，會變更作業系統超時值。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b56e609d5e3b7c92a887a98ae5d02bfbfb7a78e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848491"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更作業系統超時值。

## <a name="syntax"></a>語法

```
bootcfg /timeout <timeOutValue> [/s <computer> [/u <Domain\User>/p <Password>]]
```

### <a name="parameters"></a>參數


|        參數        |                                                                                                                                                                                  描述                                                                                                                                                                                   |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /timeout <timeOutValue> | 指定 [開機載入器] 區段中的超時值。 <timeOutValue> 是在 NTLDR 載入預設值之前，使用者必須從開機載入器畫面中選取作業系統的秒數。 <timeOutValue> 的有效範圍為0-999。 如果值為0，則 NTLDR 會立即啟動預設作業系統，而不會顯示開機載入器畫面。 |
|      /s <computer>      |                                                                                                                               指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                                                                                                               |
|    /u < 的網域 \ 使用者 >     |                                                                                       使用 <User> 所指定之使用者的帳戶許可權，或 < Domain\User > 來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。                                                                                        |
|      /p <Password>      |                                                                                                                                            指定 **/u**參數中指定之使用者帳戶的 <Password>。                                                                                                                                             |
|           /?            |                                                                                                                                                                      在命令提示字元顯示說明。                                                                                                                                                                      |

## <a name="examples"></a><a name=BKMK_examples></a>典型
下列範例會示範如何使用**bootcfg/timeout**命令：
```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
