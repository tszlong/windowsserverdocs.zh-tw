---
title: takeown
description: Takeown 命令的參考文章，可讓系統管理員復原先前拒絕之檔案的存取權。
ms.topic: reference
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 34fce5000a0a8c91123a2ebd4765bf4cbb00a289
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718235"
---
# <a name="takeown"></a>takeown

可藉由讓系統管理員成為檔案的擁有者，讓系統管理員復原之前遭拒的檔案存取。 此命令通常用於批次檔。

## <a name="syntax"></a>語法

```
takeown [/s <computer> [/u [<domain>\]<username> [/p [<password>]]]] /f <filename> [/a] [/r [/d {Y|N}]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (不要使用反斜線) 。 預設值為本機電腦。 此參數會套用至命令中指定的所有檔案和資料夾。 |
| u `[<domain>\]<username>` | 使用指定之使用者帳戶的許可權來執行腳本。 預設值為 [系統許可權]。 |
| /p `[<[password>]` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 |
| /f `<filename>` | 指定檔案名或目錄名稱模式。 指定模式時，您可以使用萬用字元 `*` 。 您也可以使用語法 `<sharename>\<filename>` 。 |
| /a | 提供系統管理員群組的擁有權，而不是目前的使用者。 如果您未指定此選項，則會將檔案擁有權提供給目前登入電腦的使用者。 |
| /r | 在指定目錄和子目錄中的所有檔案上執行遞迴作業。 |
| /d `{Y | N}` | 抑制當目前的使用者沒有指定目錄的 [ **列出資料夾** ] 許可權，而改為使用指定的預設值時，所顯示的確認提示。 適用于 **/d** 選項的有效值為：<ul><li>**Y** -取得目錄的擁有權。</li><li>**N** -略過目錄。<p>**注意**<br>您必須搭配使用此選項與 **/r** 選項。</li></ul> |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- 使用 (的混合模式 **？** **takeown**命令不支援和 **&#42;**) 。

- 使用 **takeown**刪除鎖定之後，您可能必須使用 Windows 檔案總管為自己授與您自己檔案和目錄的完整許可權，才能加以刪除。

## <a name="examples"></a>範例

若要取得名為 *Lostfile*之檔案的擁有權，請輸入：

```
takeown /f lostfile
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
