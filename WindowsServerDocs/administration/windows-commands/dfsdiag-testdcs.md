---
title: dfsdiag testdcs
description: Dfsdiag testdcs 命令的參考主題，它會檢查指定網域中的網域控制站設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0bbe47474f99edb1626e61a372b02090d3a45ee3
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992996"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag testdcs

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由在指定網域中的每個網域控制站上執行下列測試，檢查網域控制站的設定：

- 確認分散式檔案系統（DFS）命名空間服務正在執行，且其啟動類型設定為 [**自動**]。

- 檢查是否支援 NETLOGON 和 SYSvol 的網站成本參考。

- 依主機名稱和 IP 位址驗證網站關聯的一致性。

## <a name="syntax"></a>語法

```
dfsdiag /testdcs [/domain:<domain_name>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /domain`<domain_name>` | 要檢查之網域的名稱。 這是選擇性參數。 預設值是本機主機加入的本機網域。 |

## <a name="examples"></a>範例

若要確認*contoso.com*網域中的網域控制站設定，請輸入：

```
dfsdiag /testdcs /domain:contoso.com
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [dfsdiag 命令](dfsdiag.md)
