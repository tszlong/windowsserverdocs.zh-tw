---
title: nslookup set domain
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9019893d92201079fb60b820a14dda3763bafd6b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886639"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更預設網域名稱系統 (DNS) 網域名稱指定的名稱。
## <a name="syntax"></a>語法
```
set domain=<DomainName>
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<DomainName>|指定的預設 DNS 網域名稱的新名稱。 預設網域名稱是主機名稱。|
|{help &#124; ?}|顯示的簡短摘要**nslookup**子命令。|
## <a name="remarks"></a>備註
-   預設 DNS 網域名稱會附加到搜尋的要求，視狀態而定**defname**並**搜尋**選項。 DNS 網域搜尋清單包含的預設 DNS 網域的父代，如果它在其名稱中有至少兩個元件。 例如，如果 mfg.widgets.com 的預設 DNS 網域，[搜尋] 清單稱為 mfg.widgets.com 和 widgets.com。 使用**設定 srchlist**命令，以指定不同的清單並**全部設定**命令，以顯示清單。
## <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[nslookup 設定 srchlist](nslookup-set-srchlist.md)
[nslookup 將所有設定](nslookup-set-all.md)
