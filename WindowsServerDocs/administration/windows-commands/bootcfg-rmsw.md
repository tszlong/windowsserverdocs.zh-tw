---
title: bootcfg rmsw
description: 適用于 bootcfg rmsw 的 Windows 命令主題，其會移除指定作業系統專案的作業系統載入選項。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 956732f396e0fa353a8acd55953e46605a5c4200
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848441"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

移除指定之作業系統專案的作業系統載入選項。

## <a name="syntax"></a>語法
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
### <a name="parameters"></a>參數

|      參數       |                                                                                                      描述                                                                                                       |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                   指定遠端電腦的名稱或 IP 位址（請勿使用反斜線）。 預設是本機電腦。                                                   |
| /u <Domain>\\<User>  |          以 <User> 或 <Domain>\\<User>所指定使用者的帳戶許可權來執行命令。 預設為發出命令之電腦上目前登入使用者的許可權。          |
|    /p <Password>     |                                                                 指定 **/u**參數中指定之使用者帳戶的密碼。                                                                  |
|         /mm          |           從指定的 <OSEntryLineNum>中移除/maxmem 選項及其關聯的最大記憶體值。 /Maxmem 選項會指定作業系統可使用的 RAM 數量上限。            |
|         /bv          |                     從指定的 <OSEntryLineNum>移除/basevideo 選項。 /Basevideo 選項會指示作業系統使用標準 VGA 模式來安裝視頻驅動程式。                     |
|         /so          |                         從指定的 <OSEntryLineNum>中移除/sos 選項。 /Sos 選項會指示作業系統在載入時顯示裝置磁碟機名稱。                          |
|         /ng          |                         從指定的 <OSEntryLineNum>移除/noguiboot 選項。 /Noguiboot 選項會停用在 CTRL + ALT + del 登入提示字元之前出現的進度列。                          |
| /id <OSEntryLineNum> | 在從中移除 OS 載入選項的 Boot.ini 檔案 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。 |
|          /?          |                                                                                          在命令提示字元顯示說明。                                                                                          |

## <a name="examples"></a><a name=BKMK_examples></a>典型
下列範例會示範如何使用**bootcfg/rmsw**命令：
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
