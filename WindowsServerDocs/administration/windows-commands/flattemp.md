---
title: flattemp
description: Flattemp 命令的參考文章，可啟用或停用一般暫存資料夾。
ms.topic: reference
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac9a3ec390318d52d17f8e537eb10aad4bb1540c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030616"
---
# <a name="flattemp"></a>flattemp

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用或停用一般暫存資料夾。 您必須具有系統管理認證才能執行此命令。

> [!NOTE]
> 只有當您已安裝遠端桌面工作階段主機角色服務時，才能使用此命令。

## <a name="syntax"></a>語法

```
flattemp {/query | /enable | /disable}
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /query | 查詢目前的設定。 |
| /enable | 啟用一般暫存資料夾。 除非暫存資料夾位於使用者的主資料夾中，否則使用者將共用暫存資料夾。 |
| /disable | 停用一般暫存資料夾。 每個使用者的暫存資料夾都將位於個別的資料夾 (由使用者的會話識別碼) 決定。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 每位使用者都有唯一的暫存資料夾之後，請使用 `flattemp /enable` 來啟用一般暫存資料夾。

- 為多個使用者建立暫存資料夾的預設方法 (通常是由 TEMP 和 TMP 環境變數所指向，) 是使用 logonID 做為子資料夾名稱，在 **\temp** 資料夾中建立子資料夾。 例如，如果 TEMP 環境變數指向 C：\Temp，指派給使用者 logonID 4 的暫存資料夾是 C:\Temp\4。

    使用 **flattemp**，您可以直接指向 [\temp] 資料夾，防止子資料夾形成。 當您想要在主資料夾中包含使用者暫存資料夾時（無論是在遠端桌面工作階段主機 server 本機磁片磁碟機或共用網路磁碟機機上），這項功能就很有用。 `flattemp /enable*`只有當每位使用者都有個別的暫存資料夾時，才應該使用此命令。

- 如果使用者的暫存資料夾是在網路磁碟機機上，您可能會遇到應用程式錯誤。 當共用的網路磁碟機機在網路上無法存取時，就會發生這種情況。 由於應用程式的暫存檔無法存取或無法同步處理，因此它會以磁片已停止的方式回應。 不建議將暫存資料夾移至網路磁碟機機。 預設值是保留本機硬碟上的暫存資料夾。 如果您遇到非預期的行為或特定應用程式發生磁片損毀的錯誤，請將您的網路穩定，或將暫存資料夾移回本機硬碟。

- 如果您停用每個會話的個別暫存資料夾，則會忽略 **flattemp** 設定。 此選項是在遠端桌面服務設定工具中設定。

### <a name="examples"></a>範例

若要顯示一般暫存檔案夾的目前設定，請輸入：

```
flattemp /query
```

若要啟用一般暫存資料夾，請輸入：

```
flattemp /enable
```

若要停用一般暫存資料夾，請輸入：

```
flattemp /disable
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

