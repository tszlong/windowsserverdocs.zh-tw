---
title: nslookup ls
description: Nslookup ls 命令的參考文章，其中列出 DNS 網域資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c0c0aeb01cdffb1f2b779178f0298e1f120348e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936573"
---
# <a name="nslookup-ls"></a>nslookup ls

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列出 DNS 網域資訊。

## <a name="syntax"></a>語法

```
ls [<option>] <DNSdomain> [{[>] <filename>|[>>] <filename>}]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<option>` | 有效的選項包括：<ul><li>**-t：** 列出指定之類型的所有記錄。 如需詳細資訊，請參閱[nslookup set querytype](nslookup-set-querytype.md)。</li><li>**-a：** 列出 DNS 網域中的電腦別名。 此參數與 **-t CNAME**相同</li><li>**-d：** 列出 DNS 網域的所有記錄。 這個參數與 **-t ANY**相同</li><li>**-h：** 列出 DNS 網域的 CPU 和作業系統資訊。 此參數與 **-t HINFO**相同</li><li>**-s：** 列出 DNS 網域中的知名電腦服務。 這個參數與 **-t WKS**相同。 |
| `<DNSdomain>` | 指定您想要其資訊的 DNS 網域。 |
| `<filename>` | 指定要用於已儲存輸出的檔案名。 您可以使用大於（ `>` ）和 double 大於（ `>>` ）字元，以一般方式重新導向輸出。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 此命令的預設輸出包括電腦名稱稱及其相關聯的 IP 位址。

- 如果您的輸出導向至檔案，就會為每個從伺服器接收的50記錄新增雜湊標記。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set querytype](nslookup-set-querytype.md)
