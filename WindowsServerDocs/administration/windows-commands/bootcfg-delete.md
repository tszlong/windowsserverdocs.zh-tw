---
title: bootcfg delete
description: 適用于 bootcfg delete 的 Windows 命令主題，它會刪除 Boot.ini 檔案的作業系統區段中的作業系統專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01ec7a4dde1e22982f2cf0fa30245c33e09cc0ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848541"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Boot.ini 檔案的 [作業系統] 區段中刪除作業系統專案。

## <a name="syntax"></a>語法
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>參數

|         詞彙         |                                                                                             定義                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                          |
| /u <Domain>\\<User>  | 以 <User>或 <Domain>\\<User>所指定使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。 |
|    /p <Password>     |                                                        指定 **/u**參數中指定之使用者帳戶的密碼。                                                        |
| /id <OSEntryLineNum> |        在要刪除的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。        |
|          /?          |                                                                                在命令提示字元顯示說明。                                                                                 |

## <a name="examples"></a><a name=BKMK_examples></a>典型
下列範例會示範如何使用**bootcfg/delete**命令：
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
