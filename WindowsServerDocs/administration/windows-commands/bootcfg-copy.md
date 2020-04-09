---
title: bootcfg copy
description: 適用于 bootcfg copy 的 Windows 命令主題，它會建立現有開機專案的複本，您可以在其中新增命令列選項。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5194418a07aece4f15a84c3eccbc044431a865b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848681"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立現有開機專案的複本，您可以在其中新增命令列選項。

## <a name="syntax"></a>語法
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>參數

|      參數       |                                                                                             描述                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                          |
| /u <Domain>\\<User>  | 以 <User>或 <Domain>\\<User>所指定使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。 |
|    /p <Password>     |                                                        指定 **/u**參數中指定之使用者帳戶的密碼。                                                        |
|   /d <Description>   |                                                                    指定新作業系統專案的描述。                                                                    |
| /id <OSEntryLineNum> |         在要複製的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。         |
|          /?          |                                                                                在命令提示字元顯示說明。                                                                                 |

## <a name="examples"></a><a name=BKMK_examples></a>典型
下列範例會示範如何使用**bootcfg/copy**命令來複製開機專案1，並輸入 \ABC Server\\ 作為描述：
```
bootcfg /copy /d \ABC Server\ /id 1
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
