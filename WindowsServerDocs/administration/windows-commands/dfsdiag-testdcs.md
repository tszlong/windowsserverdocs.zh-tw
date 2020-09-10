---
title: dfsdiag testdcs
description: Dfsdiag testdcs 命令的參考文章，此命令會檢查指定網域中的網域控制站設定。
ms.topic: reference
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0c2796cc149d905abae56bb909d337862c5fd7f2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633857"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag testdcs

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

藉由在指定網域中的每個網域控制站上執行下列測試，檢查網域控制站的設定：

- 確認分散式檔案系統 (DFS) 命名空間服務正在執行，而且其啟動類型設定為 [ **自動**]。

- 檢查是否支援 NETLOGON 和 SYSvol 的網站成本參考。

- 驗證依主機名稱和 IP 位址的網站關聯一致性。

## <a name="syntax"></a>語法

```
dfsdiag /testdcs [/domain:<domain_name>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /domain`<domain_name>` | 要檢查之網域的名稱。 這是選擇性參數。 預設值是本機主機加入的本機網域。 |

## <a name="examples"></a>範例

若要確認 *contoso.com* 網域中的網域控制站設定，請輸入：

```
dfsdiag /testdcs /domain:contoso.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [dfsdiag 命令](dfsdiag.md)
