---
title: nslookup server
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52a846b1084380d0b40d58d81c11d20dacb407bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869149"
---
# <a name="nslookup-server"></a>nslookup server

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更指定的網域名稱系統 (DNS) 網域的預設伺服器。
## <a name="syntax"></a>語法
```
server <DNSDomain>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<DNSDomain>|必要。 指定新的 DNS 網域的預設伺服器。|
|{help &#124; ?}|顯示的簡短摘要**nslookup**子命令。|
## <a name="remarks"></a>備註
-   **Server**命令會使用目前的預設伺服器查詢指定的 DNS 網域的相關資訊。 這是相對於**lserver**命令，這個命令會使用初始的伺服器。
## <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[nslookup lserver](nslookup-lserver.md)
