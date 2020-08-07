---
title: fsutil quota
description: Fsutil quota 命令的參考文章，它會管理 NTFS 磁片區上的磁片配額，以提供更精確的網路型存放裝置控制。
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 7edf7ac908df419611fb42dd819323b15c8ded4e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889929"
---
# <a name="fsutil-quota"></a>fsutil quota

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

管理 NTFS 磁片區上的磁片配額，以更精確地控制以網路為基礎的存放裝置。

## <a name="syntax"></a>語法

```
fsutil quota [disable] <volumepath>
fsutil quota [enforce] <volumepath>
fsutil quota [modify] <volumepath> <threshold> <limit> <username>
fsutil quota [query] <volumepath>
fsutil quota [track] <volumepath>
fsutil quota [violations]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| disable | 在指定的磁片區上停用配額追蹤和強制執行。 |
| 強制執行 | 在指定的磁片區上強制執行配額使用量。 |
| 修改 | 修改現有的磁片配額，或建立新的配額。 |
| 查詢 | 列出現有的磁片配額。 |
| 追蹤 | 追蹤指定磁片區上的磁片使用量。 |
| 衝突 | 搜尋系統和應用程式記錄檔，並顯示一則訊息，指出已偵測到配額違規，或使用者已達到配額閾值或配額限制。 |
| `<volumepath>` | 必要。 指定磁片磁碟機名稱，後面接著冒號或 GUID，格式為 `volume{GUID}` 。 |
| `<threshold>`  | 設定發出警告 () 的限制（以位元組為單位）。 此為命令所需的參數 `fsutil quota modify` 。 |
| `<limit>` | 設定允許的磁片使用量上限 (以位元組為單位) 。 此為命令所需的參數 `fsutil quota modify` 。 |
| `<username>` | 指定網域或使用者名稱。 此為命令所需的參數 `fsutil quota modify` 。 |

#### <a name="remarks"></a>備註

- 磁片配額是以每個磁片區為基礎來執行，並可讓您以每個使用者為基礎來執行硬式和軟性儲存限制。

- 您可以使用使用**fsutil quota**的寫入腳本，在每次新增使用者時設定配額限制，或自動追蹤配額限制、將它們編譯成報表，以及在電子郵件中自動傳送給系統管理員。

### <a name="examples"></a>範例

若要列出以 GUID {928842df-5a01-11de-a85c-806e6f6e6963} 指定之磁片區的現有磁片配額，請輸入：

```
fsutil quota query volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

若要列出使用磁碟機號**C：** 指定之磁片區的現有磁片配額，請輸入：

```
fsutil quota query C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)
