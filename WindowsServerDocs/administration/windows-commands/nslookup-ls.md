---
title: nslookup ls
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 632d25e29c09d7a164668128196964d082e160c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848519"
---
# <a name="nslookup-ls"></a>nslookup ls

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

列出的網域名稱系統 (DNS) 網域的資訊。
## <a name="syntax"></a>語法
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<Option>|下表列出有效的選項。<br /><br />--t： 列出指定之類型的所有記錄。 如需描述<querytype>，請參閱 < **setquerytype**中其他參考資料。<br />--a： 列出的 DNS 網域中電腦的別名。 這個參數是的同義字 **-t CNAME**<br />--d： 列出的 DNS 網域的所有記錄。 這個參數是的同義字 **-t y**<br />--h： 列出的 DNS 網域的 CPU 和作業系統資訊。 這個參數是的同義字 **-t HINFO**<br />--s： 列出已知的服務中的 DNS 網域的電腦。 這個參數是的同義字 **-t WKS**。|
|<DNSDomain>|指定您想要的資訊的 DNS 網域。|
|<FileName>|指定要儲存輸出的檔案名稱。 您可以使用大於 (>) 和雙引號大於 (>>) 將輸出重新導向以一般方式的字元。|
|{help &#124; ?}|顯示的簡短摘要**nslookup**子命令。|
## <a name="remarks"></a>備註
-   預設輸出會包含電腦名稱和其 IP 位址。 雜湊標記時輸出導向至檔案時，會列印 50 個從伺服器收到的記錄
## <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[nslookup 設定 querytype](nslookup-set-querytype.md)
