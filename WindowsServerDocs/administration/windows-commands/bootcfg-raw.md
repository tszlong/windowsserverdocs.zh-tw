---
title: bootcfg raw
description: 適用於 Windows 命令主題**未經處理的 bootcfg** -新增作業系統載入選項，可指定為字串中的作業系統項目 **[operating systems]** Boot.ini 檔案區段。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5334ff8a1c5d15343b4a48814b52012c641016a4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434692"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

新增作業系統載入選項，可指定為字串中的作業系統項目 **[operating systems]** Boot.ini 檔案區段。

## <a name="syntax"></a>語法
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
## <a name="parameters"></a>參數

|         詞彙          |                                                                                                            定義                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     |                                                        指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設是本機電腦。                                                         |
| /u <Domain> \\<User>  |               使用指定的使用者帳戶權限執行命令<User>或是<Domain> \\ <User>。 預設值是目前登入的使用者發出命令的電腦上的權限。                |
|     /p <Password>     |                                                                       指定在指定的使用者帳戶的密碼 **/u**參數。                                                                       |
| <OSLoadOptionsString> | 指定作業系統載入選項，將新增至作業系統項目。 這些載入選項將會取代任何現有的作業系統項目相關聯的載入選項。 沒有驗證<OSLoadOptions>完成。 |
| /id <OSEntryLineNum>  |                       Boot.ini 檔案中，以更新 [operating systems] 區段中指定的作業系統項目行號。 [Operating systems] 區段標頭之後的第一行會是 1。                       |
|          /a           |                                                       指定要新增的作業系統選項，應該附加至任何現有的作業系統選項。                                                        |
|          /?           |                                                                                               在命令提示字元顯示說明。                                                                                                |

##### <a name="remarks"></a>備註
- **bootcfg 原始**用來將文字加入結尾的作業系統項目，並覆寫任何現有的作業系統項目選項。 此文字應該包含有效的 OS 載入選項，例如 **/debug**， **/fastdetect**， **/nodebug**， **/baudrate**， **/crashdebug**，並 **/sos**。 例如，下列命令會將" **/偵錯 /fastdetect**」 的第一個的作業系統項目結束時，以取代任何先前的作業系統項目選項：
  ```
  bootcfg /raw "/debug /fastdetect" /id 1
  ```
  ## <a name="BKMK_examples"></a>範例
  下列範例示範如何使用**bootcfg / 原始**命令：
  ```
  bootcfg /raw "/debug /sos" /id 2
  bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 "/crashdebug " /id 2
  ```
  #### <a name="additional-references"></a>其他參考資料
  [命令列語法關鍵](command-line-syntax-key.md)
