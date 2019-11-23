---
title: nslookup ls
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ecc419a72599b661865af6283821129a7021938a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373094"
---
# <a name="nslookup-ls"></a>nslookup ls

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列出網域名稱系統（DNS）網域的資訊。
## <a name="syntax"></a>語法
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
## <a name="parameters"></a>Parameters

|    參數    |                                                                                                                                                                                                                                                                                                               描述                                                                                                                                                                                                                                                                                                                |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Option>     | 下表列出有效的選項。<br /><br />--t：列出指定之類型的所有記錄。 如需 <querytype>的描述，請參閱其他參考中的**setquerytype** 。<br />--a：列出 DNS 網域中電腦的別名。 此參數是 **-t CNAME**的同義字<br />--d：列出 DNS 網域的所有記錄。 此參數是 **-t ANY**的同義字<br />--h：列出 DNS 網域的 CPU 和作業系統資訊。 此參數是 **-t HINFO**的同義字<br />--s：列出 DNS 網域中的知名電腦服務。 此參數是 **-t WKS**的同義字。 |
|   <DNSDomain>   |                                                                                                                                                                                                                                                                                         指定您想要其資訊的 DNS 網域。                                                                                                                                                                                                                                                                                         |
|   <FileName>    |                                                                                                                                                                                                                                 指定用來儲存輸出的檔案名。 您可以使用大於（>）和 double 大於（> >）字元，以平常的方式重新導向輸出。                                                                                                                                                                                                                                  |
| {help &#124; ？} |                                                                                                                                                                                                                                                                                          顯示**nslookup**子命令的簡短摘要。                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>備註
- 預設輸出包含電腦名稱稱和其 IP 位址。 當輸出導向至檔案時，會針對每個從伺服器接收的50記錄列印雜湊標記
  ## <a name="additional-references"></a>其他參考
  [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup set querytype](nslookup-set-querytype.md)
