---
title: nslookup ls
description: Nslookup ls 命令的參考文章，此命令會列出 DNS 網域資訊。
ms.topic: reference
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6910fa753ff394416f1cbf201db690647cebe9a0
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635687"
---
# <a name="nslookup-ls"></a>nslookup ls

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列出 DNS 網域資訊。

## <a name="syntax"></a>語法

```
ls [<option>] <DNSdomain> [{[>] <filename>|[>>] <filename>}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<option>` | 有效的選項包括：<ul><li>**-t：** 列出指定之類型的所有記錄。 如需詳細資訊，請參閱 [nslookup set querytype](nslookup-set-querytype.md)。</li><li>**-a：** 列出 DNS 網域中電腦的別名。 此參數與 **-t CNAME**相同</li><li>**-d：** 列出 DNS 網域的所有記錄。 此參數與 **-t ANY**相同</li><li>**-h：** 列出 DNS 網域的 CPU 和作業系統資訊。 此參數與 **-t HINFO**相同</li><li>**-s：** 列出 DNS 網域中知名的電腦服務。 此參數與 **-t WKS**相同。 |
| `<DNSdomain>` | 指定您要取得資訊的 DNS 網域。 |
| `<filename>` | 指定要用於儲存之輸出的檔案名。 您可以使用 [大於 (] `>`) ，然後按兩下 [大於] (`>>`) 字元，以一般方式重新導向輸出。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 此命令的預設輸出會包含電腦名稱稱及其相關聯的 IP 位址。

- 如果您的輸出被導向至檔案，則會為每個從伺服器收到的50記錄新增雜湊標記。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set querytype](nslookup-set-querytype.md)
