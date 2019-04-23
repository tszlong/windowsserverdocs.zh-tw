---
title: takeown
description: 了解如何藉由成為檔案的擁有者取得檔案的存取權。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b5a4874edf9fa4406d4643e686fed2b725699dd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854359"
---
# <a name="takeown"></a>takeown

可藉由讓系統管理員成為檔案的擁有者，讓系統管理員復原之前遭拒的檔案存取。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
takeown [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <File name> [/a] [/r [/d {Y|N}]]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/s\<電腦 >|指定的名稱或遠端電腦的 IP 位址 （不使用反斜線）。 預設值為本機電腦。 此參數適用於所有的檔案和命令中指定的資料夾。|
|/u [\<網域 >\]<User name>|使用指定的使用者帳戶的權限執行指令碼。 預設值是系統權限。|
|/p [\<Password>]|指定在指定的使用者帳戶的密碼 **/u**參數。|
|/f\<檔案名稱 >|指定的檔案名稱或目錄名稱模式。 您可以使用萬用字元 * 指定模式時。 您也可以使用語法*ShareName*\*FileName *。|
|/a|擁有權提供給系統管理員群組，而不是目前的使用者。|
|/r|執行指定的目錄和子目錄中的遞迴作業上的所有檔案。|
|/d {Y \| N}|隱藏目前的使用者指定的目錄中，沒有 「 列出資料夾 」 權限，並改為使用指定的預設值時，會顯示確認提示。 有效值 **/d**選項如下所示：</br>-Y:取得目錄的擁有權。</br>-N:略過該目錄。</br>請注意，您必須使用此選項搭配 **/r**選項。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   此命令通常用於批次檔。
-   如果 **/a**參數未指定，檔案擁有權指定給目前登入電腦的使用者。
-   使用混合的模式 (**嗎？** 並 **&#42;**) 不會受到**takeown**命令。
-   刪除具有鎖定之後**takeown**，您可能必須使用 Windows 檔案總管或**cacls**命令來授與您自己的完整權限檔案和目錄之前，您也可以將它們刪除。 如需詳細資訊**cacls**，請參閱本主題結尾處的 「 其他參照 」。

## <a name="BKMK_examples"></a>範例

若要取得名為 Lostfile 檔案的擁有權，請輸入：
```
takeown /f lostfile
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)