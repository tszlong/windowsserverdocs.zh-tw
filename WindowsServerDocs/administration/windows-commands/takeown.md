---
title: takeown
description: 瞭解如何藉由成為檔案的擁有者來取得檔案的存取權。
ms.topic: reference
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b87f773f1b42291a679a642793f2b534982164d2
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027196"
---
# <a name="takeown"></a>takeown

可藉由讓系統管理員成為檔案的擁有者，讓系統管理員復原之前遭拒的檔案存取。



## <a name="syntax"></a>語法

```
takeown [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <File name> [/a] [/r [/d {Y|N}]]
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/s \<Computer>|指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設值為本機電腦。 此參數會套用至命令中指定的所有檔案和資料夾。|
|u-sql\<Domain>\]<User name>|使用指定之使用者帳戶的許可權來執行腳本。 預設值為 [系統許可權]。|
|/p [ \<Password> ]|指定 **/u** 參數中指定之使用者帳戶的密碼。|
|/f \<File name>|指定檔案名或目錄名稱模式。 指定模式時，您可以使用萬用字元 *。 您也可以使用「*共用名稱* \* 名稱 *」語法。|
|/a|提供系統管理員群組的擁有權，而不是目前的使用者。|
|/r|在指定目錄和子目錄中的所有檔案上執行遞迴作業。|
|/d {Y \| N}|抑制當目前的使用者沒有指定目錄的「列出資料夾」許可權，而改為使用指定的預設值時，所顯示的確認提示。 適用于 **/d** 選項的有效值如下所示：</br>-Y：取得目錄的擁有權。</br>-N：略過目錄。</br>請注意，您必須搭配使用此選項與 **/r** 選項。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   此命令通常用於批次檔中。
-   如果未指定 **/a** 參數，則會將檔案擁有權提供給目前登入電腦的使用者。
-   使用 (的混合模式 **？** **takeown**命令不支援和 **&#42;**) 。
-   使用 **takeown**刪除鎖定之後，您可能必須使用 Windows 檔案總管或 **cacls** 命令，為自己授與您自己檔案和目錄的完整許可權，才能加以刪除。 如需有關 **cacls**的詳細資訊，請參閱本主題結尾的「其他參考」。

## <a name="examples"></a><a name="BKMK_examples"></a>範例

若要取得名為 Lostfile 之檔案的擁有權，請輸入：
```
takeown /f lostfile
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)