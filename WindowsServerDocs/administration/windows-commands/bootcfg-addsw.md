---
title: bootcfg addsw
description: 適用于 bootcfg addsw 命令的參考文章，可為指定的作業系統專案新增作業系統載入選項。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9d54c2cfdf898e1162d804220ae6dbb4a446fc5
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926353"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

為指定的作業系統專案新增作業系統載入選項。

## <a name="syntax"></a>語法

```
bootcfg /addsw [/s <computer> [/u <domain>\<user> /p <password>]] [/mm <maximumram>] [/bv] [/so] [/ng] /id <osentrylinenum>
```

### <a name="parameters"></a>參數

| 詞彙 | 定義 |
| ---- | ---------- |
| `/s <computer>` | 指定遠端電腦的名稱或 IP 位址（不要使用反斜線）。 預設是本機電腦。 |
| `/u <domain>\<user>`  | 以或指定之使用者的帳戶許可權來執行命令 `<user>` `<domain>\<user>` 。 預設為發出命令之電腦上目前登入使用者的許可權。 |
| `/p <password>` | 指定 **/u**參數中指定之使用者帳戶的密碼。 |
| `/mm <maximumram>` | 指定作業系統可使用的 RAM 數量上限（以 mb 為單位）。 值必須等於或大於 32 Mb。 |
| /bv | 將 **/basevideo**選項新增至指定的 `<osentrylinenum>` ，將作業系統導向使用已安裝之視頻驅動程式的標準 VGA 模式。 |
| /so | 將 **/sos**選項新增至指定的 `<osentrylinenum>` ，指示作業系統在載入時顯示裝置磁碟機名稱。 |
| /ng | 將 **/noguiboot**選項新增至指定的 `<osentrylinenum>` ，並停用在 CTRL + ALT + DEL 登入提示字元之前出現的進度列。 |
| `/id <osentrylinenum>` | 在新增作業系統載入選項的 Boot.ini 檔案的 [作業系統] 區段中，指定作業系統專案行號。 [作業系統] 區段標頭後面的第一行是1。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要使用**bootcfg/addsw**命令：

```
bootcfg /addsw /mm 64 /id 2
bootcfg /addsw /so /id 3
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2
bootcfg /addsw /ng /id 2
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bootcfg 命令](bootcfg.md)
