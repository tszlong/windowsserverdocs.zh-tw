---
title: finger
description: Finger 命令的參考文章，其會顯示執行手指服務或背景程式之指定遠端電腦上的使用者相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd629374b601686e91e5238ae8db060e0b6bf0f8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922420"
---
# <a name="finger"></a>finger

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示執行手指服務或背景程式之指定遠端電腦（通常是執行 UNIX 的電腦）上的使用者相關資訊。 遠端電腦會指定使用者資訊顯示的格式和輸出。 使用時不含參數，**手指**會顯示說明。

> [!IMPORTANT]
> 只有當網際網路通訊協定（TCP/IP）通訊協定是在網路連線的網路介面卡內容中安裝為元件時，才可以使用此命令。

## <a name="syntax"></a>語法

```
finger [-l] [<user>] [@<host>] [...]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| -l | 以長清單格式顯示使用者資訊。 |
| `<user>` | 指定您想要其資訊的使用者。 如果您省略*user*參數，此命令會顯示指定電腦上所有使用者的相關資訊。 |
| `@<host>` | 指定在您要尋找使用者資訊的遠端電腦上執行手指服務。 您可以指定電腦名稱稱或 IP 位址。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 您必須在**手指**參數前面加上連字號（-），而不是斜線（/）。

- `user@host`可以指定多個參數。

### <a name="examples"></a>範例

若要在電腦*users.microsoft.com*上顯示*user1*的資訊，請輸入：

```
finger user1@users.microsoft.com
```

若要顯示電腦*users.microsoft.com*上*所有使用者*的資訊，請輸入：

```
finger @users.microsoft.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
