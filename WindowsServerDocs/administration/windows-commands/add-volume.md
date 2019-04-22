---
title: 新增磁碟區
description: 適用於 Windows 命令主題**新增磁碟區**-將磁碟區加入至集合，這是集合的磁碟區陰影複製的陰影複製。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8960ffafdf49d4512e1df2dfcc046bdfbe56e224
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819469"
---
# <a name="add-volume"></a>新增磁碟區



將磁碟區陰影複製設定，也就是要進行陰影複製磁碟區組。 此命令，才能建立陰影複製。 如果未指定參數，使用**新增磁碟區**在命令提示字元中顯示說明。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
add volume <Volume> [provider <ProviderID>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<磁碟區 >|指定要加入陰影複製設定的磁碟區。 至少一個磁碟區，才能建立陰影複製。|
|[提供者\<ProviderID >]|指定要用來建立陰影複製的已註冊的提供者的提供者識別碼。 如果**提供者**未指定，會使用預設提供者。|

## <a name="remarks"></a>備註

-   新增磁碟區一次。
-   每次新增一個磁碟區時，它會檢查以確保 VSS 支援建立該磁碟區的陰影複製。 這個主要的檢查可能會失效，不過，稍後使用**設定內容**命令。
-   建立陰影複製時，環境變數連結別名至陰影識別碼，因此別名可用來編寫指令碼。

## <a name="BKMK_examples"></a>範例

若要檢視目前已註冊的提供者的清單在`DISKSHADOW>`提示中，輸入：
```
list providers
```
下列的輸出會顯示為單一的提供者，預設將使用：
```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```
將 C 磁碟機新增至陰影複製設定，並指派別名命名 nic:1，型別：
```
add volume c: alias System1
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)