---
title: bootcfg addsw
description: 適用于**bootcfg addsw**的 Windows 命令主題-新增指定之作業系統專案的作業系統載入選項。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2dd727c839babe1ae4f7743285844f35cf5bf76e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380176"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

為指定的作業系統專案新增作業系統載入選項。

## <a name="syntax"></a>語法
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parameters

|         詞彙         |                                                                                                            定義                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                        指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                                        |
| /u <Domain>\\<User>  |               以 <User> 或 <Domain>\\<User>所指定使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。               |
|    /p <Password>     |                                                                      指定 **/u**參數中指定之使用者帳戶的密碼。                                                                       |
|   /mm <MaximumRAM>   |                                          指定作業系統可使用的 RAM 數量上限（以 mb 為單位）。 值必須等於或大於 32 Mb。                                          |
|         /bv          |                                    將 **/basevideo**選項新增至指定的 <OSEntryLineNum>，並將作業系統導向至已安裝之視頻驅動程式的標準 VGA 模式。                                     |
|         /so          |                                      將 **/sos**選項新增至指定的*OSEntryLineNum*，並在載入時，引導作業系統顯示裝置磁碟機名稱。                                      |
|         /ng          |                                         將 **/noguiboot**選項新增至指定的 <OSEntryLineNum>，並停用在 CTRL + ALT + del 登入提示字元之前出現的進度列。                                          |
| /id <OSEntryLineNum> | 在新增作業系統載入選項的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。 |
|          /?          |                                                                                               在命令提示字元顯示說明。                                                                                               |

## <a name="BKMK_examples"></a>典型
下列範例會示範如何使用**bootcfg/addsw**命令：
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>其他參考
[命令列語法關鍵](command-line-syntax-key.md)
