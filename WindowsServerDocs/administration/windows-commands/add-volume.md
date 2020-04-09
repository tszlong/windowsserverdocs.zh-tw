---
title: 新增磁片區
description: '[**新增磁片**區] 的 Windows 命令主題，它會將磁片區新增至陰影複製組，這是要陰影複製的一組磁片區。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 806ab273dbb63eb7341520f56a07691fe3fac214
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851351"
---
# <a name="add-volume"></a>新增磁片區

將磁片區新增至陰影複製組，這是要陰影複製的一組磁片區。 此為建立陰影複製的必要命令。 如果使用時不含參數， **add volume**會在命令提示字元中顯示說明。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
add volume <Volume> [provider <ProviderID>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
| `<Volume>` | 指定要加入陰影複製組的磁片區。 必須至少有一個磁片區，才能建立陰影複製。|
| `[provider \<ProviderID>]` | 指定要用來建立陰影複製之已註冊提供者的提供者識別碼。 如果未指定**provider** ，則會使用預設提供者。|

## <a name="remarks"></a>備註

-   磁片區會一次新增一個。

-   每次新增磁片區時，就會進行檢查，以確保 VSS 支援該磁片區的陰影複製建立。 不過，此主要檢查可能會因為稍後使用**set coNtext**命令而失效。

-   建立陰影複製時，環境變數會將別名連結到陰影識別碼，因此別名可以用於腳本處理。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要查看目前已註冊的提供者清單，請在 `DISKSHADOW>` 提示字元中輸入：

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

若要將磁片磁碟機 C 新增至陰影複製組，並指派名為 System1 的別名，請輸入：

```
add volume c: alias System1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)