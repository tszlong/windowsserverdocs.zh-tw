---
title: nslookup set root
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63bdd16263c64f823530119754c31de24e395159
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820789"
---
# <a name="nslookup-set-root"></a>nslookup set root

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更查詢所用的根伺服器的名稱。
## <a name="syntax"></a>語法
```
set root=<RootServer>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<RootServer>|指定根伺服器的新名稱。 預設值是 ns.nic.ddn.mil。|
|{help &#124; ?}|顯示的簡短摘要**nslookup**子命令。|
## <a name="remarks"></a>備註
-   **組根**子命令會影響**根**子命令。
## <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[nslookup 根](nslookup-root.md)
