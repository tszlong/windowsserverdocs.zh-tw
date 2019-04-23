---
title: 新增
description: 適用於 Windows 命令主題**add_1** -將磁碟區新增至包含要進行陰影複製，磁碟區組，或將別名新增至 「 別名 」 環境。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 549c8560774f004a60926ce568c850fd1b71c7f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889949"
---
# <a name="add"></a>新增


將磁碟區新增至包含要進行陰影複製，磁碟區組，或將別名新增至 「 別名 」 環境。 如果沒有子命令，使用**新增**列出目前的磁碟區和別名。

> [!NOTE]
> 在建立陰影複製之前，別名不會新增至 「 別名 」 環境。 應該使用 新增別名，您需要立即**新增別名**。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
add 
add volume <Volume> [provider <ProviderID>] 
add alias <AliasName> <AliasValue>
```

## <a name="add-subcommands"></a>新增子命令

|子命令|描述|
|----------|-----------|
|音量|將磁碟區陰影複製設定，也就是要進行陰影複製磁碟區組。 請參閱[新增磁碟區](add-volume.md)語法和參數。|
|別名|將指定的名稱和值加入至 「 別名 」 環境中。 請參閱[新增別名](add-alias.md)語法和參數。|
|/?|在命令列中顯示說明。|

## <a name="BKMK_examples"></a>範例

若要顯示新增的磁碟區和目前環境中的別名，請輸入：
```
add
```
下列的輸出會顯示 C 磁碟機，已加入至陰影複製設定：
```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)