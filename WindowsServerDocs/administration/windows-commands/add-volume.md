---
title: 新增磁片區
description: Add volume 命令的參考主題，它會將磁片區新增到陰影複製組，這是要陰影複製的一組磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8cfd3d8f7d9f008e3136d8f694dc00370b8b0f2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719203"
---
# <a name="add-volume"></a>新增磁片區

將磁片區新增至陰影複製組，這是要陰影複製的一組磁片區。 建立陰影複製時，環境變數會將別名連結到陰影識別碼，因此別名可以用於腳本處理。

磁片區會一次新增一個。 每次新增磁片區時，會進行檢查以確定 VSS 支援該磁片區的陰影複製建立。 這項檢查可能會在稍後使用**set coNtext**命令而失效。

此為建立陰影複製的必要命令。 如果使用時不含參數， **add volume**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
add volume <volume> [provider <providerid>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<volume>` | 指定要加入陰影複製組的磁片區。 必須至少有一個磁片區，才能建立陰影複製。 |
| `[provider \<providerid>]` | 指定要用來建立陰影複製之已註冊提供者的提供者識別碼。 如果未指定**provider** ，則會使用預設提供者。 |

## <a name="examples"></a>範例

若要查看目前已註冊的提供者清單， `diskshadow>`請在命令提示字元中輸入：

```
list providers
```

下列輸出會顯示單一提供者，預設將會用到：

```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```

若要將磁片磁碟機 C：新增至陰影複製組，並指派名為*System1*的別名，請輸入：

```
add volume c: alias System1
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [設定內容命令](set-context.md)
