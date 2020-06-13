---
title: nslookup set type
description: Nslookup set type 命令的參考主題，它會變更查詢的資源記錄類型。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5248e314-fac1-413e-81dc-bbe0a0873ba5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 793ab40bbc47df09eb71642623e42a6f65c56842
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721351"
---
# <a name="nslookup-set-type"></a>nslookup set type

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更查詢的資源記錄類型。 如需資源記錄類型的詳細資訊，請參閱[徵求意見（Rfc） 1035](https://tools.ietf.org/html/rfc1035)。

> [!NOTE]
> 此命令與[nslookup set querytype](nslookup-set-querytype.md)命令相同。

## <a name="syntax"></a>語法

```
set type=<resourcerecordtype>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<resourcerecordtype>` | 指定 DNS 資源記錄類型。 預設的資源記錄類型為 **，但**您可以使用下列任何值：<ul><li>**答：** 指定電腦的 IP 位址。</li><li>**任何：** 指定電腦的 IP 位址。</li><li>**CNAME：** 指定別名的標準名稱。</li><li>**GID**指定組名的群組識別碼。</li><li>**HINFO：** 指定電腦的 CPU 和類型的作業系統。</li><li>**MB：** 指定信箱的功能變數名稱。</li><li>**MG：** 指定郵件群組成員。</li><li>**MINFO：** 指定信箱或郵寄清單資訊。</li><li>**MR：** 指定郵件重新命名功能變數名稱。</li><li>**MX：** 指定郵件交換器。</li><li>**NS：** 指定命名區域的 DNS 名稱伺服器。</li><li>**PTR：** 如果查詢是 IP 位址，則指定電腦名稱稱;否則，指定其他資訊的指標。</li><li>**SOA：** 指定 DNS 區域的啟動許可權。</li><li>**TXT：** 指定文字資訊。</li><li>**UID：** 指定使用者識別碼。</li><li>**UINFO：** 指定使用者資訊。</li><li>**WKS：** 描述知名的服務。</li></ul> |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set type](nslookup-set-querytype.md)
