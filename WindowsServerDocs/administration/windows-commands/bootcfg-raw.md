---
title: bootcfg raw
description: 適用于 bootcfg raw 的 Windows 命令主題，其會將作業系統載入選項（指定為字串）新增至 Boot.ini 檔案之作業系統區段中的作業系統專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd5137187c5ba1dc1b410d728f1c2930ddcbc3cc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848511"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將指定為字串的作業系統載入選項新增至 Boot.ini 檔案的 **[作業系統]** 區段中的作業系統專案。

## <a name="syntax"></a>語法
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
### <a name="parameters"></a>參數

|         詞彙          |                                                                                                            定義                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     |                                                        指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                                         |
| /u <Domain> \\<User>  |               以 <User> 或 <Domain>\\<User>所指定使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。                |
|     /p <Password>     |                                                                       指定 **/u**參數中指定之使用者帳戶的密碼。                                                                       |
| <OSLoadOptionsString> | 指定要新增至作業系統專案的作業系統載入選項。 這些載入選項會取代任何與作業系統專案相關聯的現有載入選項。 未進行 <OSLoadOptions> 的驗證。 |
| /id <OSEntryLineNum>  |                       在要更新的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。                       |
|          /a           |                                                       指定要新增的作業系統選項應附加至任何現有的作業系統選項。                                                        |
|          /?           |                                                                                               在命令提示字元顯示說明。                                                                                                |

##### <a name="remarks"></a>備註
- **bootcfg raw**是用來將文字新增至作業系統專案的結尾，並覆寫任何現有的作業系統專案選項。 此文字應包含有效的 OS 載入選項，例如 **/debug**、 **/fastdetect**、 **/nodebug**、 **/baudrate**、 **/crashdebug**和 **/sos**。 例如，下列命令會將 **/debug/fastdetect**新增至第一個作業系統專案的結尾，並取代任何先前的作業系統專案選項：
  ```
  bootcfg /raw /debug /fastdetect /id 1
  ```
  ## <a name="examples"></a><a name=BKMK_examples></a>典型
  下列範例會示範如何使用**bootcfg/raw**命令：
  ```
  bootcfg /raw /debug /sos /id 2
  bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 /crashdebug  /id 2
  ```
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法關鍵](command-line-syntax-key.md)
