---
title: bootcfg addsw
description: 適用于 bootcfg addsw 的 Windows 命令主題，會為指定的作業系統專案新增作業系統載入選項。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9ae5175dfc3b068276f6ab95d6823699c96b2b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848711"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

為指定的作業系統專案新增作業系統載入選項。

## <a name="syntax"></a>語法
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
### <a name="parameters"></a>參數

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

## <a name="examples"></a><a name=BKMK_examples></a>典型
下列範例會示範如何使用**bootcfg/addsw**命令：
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
