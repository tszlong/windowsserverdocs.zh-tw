---
title: bootcfg query
description: 適用于**bootcfg 查詢**的 Windows 命令主題：查詢，並顯示 boot.ini 的 [開機載入器] 和 [作業系統] 區段專案。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae82357cfe178343872448c2ebd46c49a797b5a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379906"
---
# <a name="bootcfg-query"></a>bootcfg query

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

查詢並顯示來自 Boot.ini 的 [開機載入器] 和 [作業系統] 區段專案。

## <a name="syntax"></a>語法
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
## <a name="parameters"></a>參數

|        詞彙         |                                                                                             定義                                                                                              |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>    |                                         指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                          |
| /u <Domain> @ no__t-1 @ no__t-2 | 以 <User>or <Domain> @ no__t-2 @ no__t-3 所指定使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。 |
|    /p <Password>    |                                                        指定 **/u**參數中指定之使用者帳戶的密碼。                                                        |
|         /?          |                                                                                在命令提示字元顯示說明。                                                                                 |

##### <a name="remarks"></a>備註
- 以下是**bootcfg/query**輸出的範例：
  ```
  Boot Loader Settings
  ----------
  timeout: 30
  default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  Boot Entries
  ------
  Boot entry ID:   1
  Friendly Name:   ""
  path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  OS Load Options: /fastdetect /debug /debugport=com1:
  ```
- [ **Bootcfg 查詢**輸出] 的 [開機載入器設定] 部分會顯示 boot.ini 的 [開機載入器] 區段中的每個專案。
- **Bootcfg 查詢**輸出的開機專案部分會針對 boot.ini 的 [作業系統] 區段中的每個作業系統專案顯示下列詳細資料：[開機專案識別碼]、[易記名稱]、[路徑] 和 [OS 載入選項]。
  ## <a name="BKMK_examples"></a>典型
  下列範例會示範如何使用**bootcfg/query**命令：
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  #### <a name="additional-references"></a>其他參考
  [命令列語法關鍵](command-line-syntax-key.md)
