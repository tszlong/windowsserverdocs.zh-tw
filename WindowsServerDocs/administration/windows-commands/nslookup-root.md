---
title: nslookup root
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc2952bdbf709c31d720a7fb57430429edf9feb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871909"
---
# <a name="nslookup-root"></a>nslookup root

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更網域名稱系統 (DNS) 網域命名空間的根伺服器的預設伺服器。
## <a name="syntax"></a>語法
```
root 
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|{help &#124; ?}|顯示的簡短摘要**nslookup**子命令。|
## <a name="remarks"></a>備註
-   目前，使用 ns.nic.ddn.mil 名稱伺服器。 此命令為 lserver ns.nic.ddn.mil 的同義字。 您可以變更與根伺服器的名稱**組根**命令。
## <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[nslookup 設定根目錄](nslookup-set-root.md)
