---
title: 自動掛接
description: 自動掛接命令的參考文章，可啟用或停用自動掛接功能。
ms.topic: article
ms.assetid: 4635fc91-a477-4f17-8dcc-aa08854bfe45
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a31e1569f1db4f47c63aae2e859587f5088dcbc4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895191"
---
# <a name="automount"></a>自動掛接

適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

- [命令列語法關鍵](command-line-syntax-key.md)

> [!IMPORTANT]
> 在 [存放區域網路] ([SAN) 設定] 中，停用 [自動掛接] 可防止 Windows 將磁碟機號自動掛接或指派給系統可看到的任何新基本磁碟區。

## <a name="syntax"></a>語法

自動掛接 [{enable | disable | 清理}] [noerr]

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| enable | 可讓 Windows 自動掛接新增至系統的新基本和動態磁碟區，並將它們指派給磁碟機號。 |
| disable | 防止 Windows 自動裝載新增至系統的任何新的基本和動態磁碟區。<p>**注意**：停用自動掛接可能會導致容錯移轉叢集無法通過 [驗證設定] Wizard 的儲存部分。 |
| scrub | 移除磁碟區掛接點目錄，並登錄不再位於系統中之磁碟區的設定。 這防止了自動掛接先前在系統中的磁碟區，並在它們重新新增回系統時，給予它們先前的磁碟區掛接點。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="examples"></a>範例

若要查看自動掛接功能是否已啟用，請在 diskpart 命令中輸入下列命令：

```
automount
```

若要啟用自動掛接功能，請輸入：

```
automount enable
```

若要停用自動掛接功能，請輸入：

```
automount disable
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [diskpart 命令](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc770877(v%3dws.11))
