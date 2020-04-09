---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: Fsutil 配額
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4bb320d9192848cd7a6719c58bde4111798a799e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844211"
---
# <a name="fsutil-quota"></a>Fsutil 配額
>適用于： Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

管理 NTFS 磁片區上的磁片配額，以更精確地控制以網路為基礎的存放裝置。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil quota [disable] <VolumePath>
fsutil quota [enforce] <VolumePath>
fsutil quota [modify] <VolumePath> <Threshold> <Limit> <UserName>
fsutil quota [query] <VolumePath>
fsutil quota [track] <VolumePath>
fsutil quota [violations]
```

### <a name="parameters"></a>參數

|   參數   |                                                                                    描述                                                                                    |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    停用    |                                                         在指定的磁片區上停用配額追蹤和強制執行。                                                          |
|    強制執行    |                                                                   在指定的磁片區上強制執行配額使用量。                                                                   |
|    modify     |                                                              修改現有的磁片配額，或建立新的配額。                                                              |
|     query     |                                                                            列出現有的磁片配額。                                                                            |
|     指點     |                                                                    追蹤指定磁片區上的磁片使用量。                                                                     |
|  衝突   | 搜尋系統和應用程式記錄檔，並顯示一則訊息，指出已偵測到配額違規，或使用者已達到配額閾值或配額限制。 |
| \<VolumePath > |                                  必要。 指定磁片磁碟機名稱，後面接著冒號或格式為**磁片區 {** <em>GUID</em> **}** 的 GUID。                                  |
| \<閾值 >  |                            設定發出警告的限制（以位元組為單位）。 這是**fsutil quota modify**命令所需的參數。                            |
|   \<限制 >    |                                設定允許的磁片使用量上限（以位元組為單位）。 這是**fsutil quota modify**命令所需的參數。                                |
|  \<使用者名稱 >  |                                      指定網域或使用者名稱。 這是**fsutil quota modify**命令所需的參數。                                       |

## <a name="remarks"></a>備註

-   磁片配額是以每個磁片區為基礎來執行，並可讓您以每個使用者為基礎來執行硬式和軟性儲存限制。

-   您可以使用使用**fsutil quota**的寫入腳本，在每次新增使用者時設定配額限制，或自動追蹤配額限制、將它們編譯成報表，以及在電子郵件中自動傳送給系統管理員。

### <a name="examples"></a><a name="BKMK_examples"></a>典型
若要列出以 GUID {928842df-5a01-11de-a85c-806e6f6e6963} 指定之磁片區的現有磁片配額，請輸入：

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

若要列出使用磁碟機號**C：** 指定之磁片區的現有磁片配額，請輸入：

```
Fsutil quota query C:
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


