---
title: tpmvscmgr
description: Tpmvscmgr 的參考文章，這是一種命令列工具，可讓具有系統管理認證的使用者在電腦上建立及刪除 TPM 虛擬智慧卡。
ms.topic: reference
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1dfa4a51c0ea75092e3abf885476e77d3c35435a
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156427"
---
# <a name="tpmvscmgr"></a>tpmvscmgr

Tpmvscmgr 命令列工具可讓具有系統管理認證的使用者在電腦上建立及刪除 TPM 虛擬智慧卡。

## <a name="syntax"></a>Syntax

```
tpmvscmgr create [/name] [/adminkey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```

```
tpmvscmgr destroy [/instance <instanceID>] [/?]
```

### <a name="create-parameters"></a>建立參數

**Create**命令會在使用者的系統上設定新的虛擬智慧卡。 如果需要刪除，它也會傳回新建立之卡片的實例識別碼以供稍後參考。 實例識別碼的格式為 **ROOT\SMARTCARDREADER\000n** ，其中 **n** 從0開始，每次建立新的虛擬智慧卡時，會增加1。

| 參數 | 描述 |
|--|--|
| /name | 必要。 表示新的虛擬智慧卡名稱。 |
| /adminkey | 指出如果使用者忘記 PIN，可用於重設卡片 PIN 的所需系統管理員金鑰。 此可包括：<ul><li>**預設** 值-指定 *010203040506070801020304050607080102030405060708*的預設值。</li><li>**提示** ：提示使用者輸入系統管理員金鑰的值。</li><li>**隨機** -針對未傳回給使用者的卡片，以隨機設定的方式產生系統管理員金鑰。 這會使用智慧卡管理工具來建立可能無法管理的卡片。 使用隨機選項時，系統管理員金鑰必須輸入為48的十六進位字元。</li></ul> |
| /PIN | 指出所需的使用者 PIN 值。<ul><li>**預設** 值-指定預設的 PIN 碼12345678。</li><li>**提示** ：提示使用者在命令列輸入 PIN。 PIN 必須至少為八個字元，且可包含數位、字元和特殊字元。</li></ul> |
| /PUK | 指出所需的 PIN 解除鎖定金鑰 (PUK) 值。 PUK 值的長度必須至少為八個字元，且可包含數位、字元和特殊字元。 如果省略此參數，則會建立不含 PUK 的卡片。 選項包括：<ul><li>**預設** 值-指定 *12345678*的預設 PUK。</li><li>**提示** ：提示使用者在命令列輸入 PUK。</li></ul> |
| /generate | 在儲存體中產生虛擬智慧卡運作所需的檔案。 如果您未使用 **/generate** 參數，就像您在沒有基礎檔案系統的情況下建立卡片一樣。 沒有檔案系統的卡片只能由智慧卡管理系統（例如 Microsoft Configuration Manager）進行管理。 |
| /machine | 可讓您指定可建立虛擬智慧卡的遠端電腦名稱稱。 這只能在網域環境中使用，而且會依賴 DCOM。 若要讓命令在不同的電腦上成功建立虛擬智慧卡，執行此命令的使用者必須是遠端電腦上本機系統管理員群組的成員。 |
| /? | 顯示此命令的說明。 |

### <a name="destroy-parameters"></a>終結參數

終結 **命令會** 安全地刪除使用者電腦的虛擬智慧卡。

> [!WARNING]
> 如果虛擬智慧卡已刪除，就無法復原。

| 參數 | 描述 |
|--|--|
| /instance | 指定要移除之虛擬智慧卡的實例識別碼。 建立卡片時 tpmvscmgr.exe 會將 instanceID 產生為 output。 **/Instance**參數是損**毀**命令的必要欄位。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 若為英數位元輸入，則允許完整的127字元 ASCII 組。

## <a name="examples"></a>範例

若要建立可在稍後由另一部電腦啟動的智慧卡管理工具管理的虛擬智慧卡，請輸入：

```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey DEFAULT /PIN PROMPT
```

或者，您可以在命令列建立系統管理員金鑰，而不是使用預設的系統管理員金鑰。 下列命令顯示如何建立系統管理員金鑰。

```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey PROMPT /PIN PROMPT
```

若要建立可用於註冊憑證的非受控虛擬智慧卡，請輸入：

```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey RANDOM /PIN PROMPT /generate
```

系統會以隨機系統管理員金鑰來建立虛擬智慧卡。 建立卡片之後，會自動捨棄金鑰。 這表示，如果使用者忘記 PIN 或想要變更 PIN，則使用者需要刪除卡片並重新建立。

若要刪除卡片，請輸入：

```
tpmvscmgr.exe destroy /instance <instance ID>
```

其中 `<instanceID>` 是使用者建立卡片時，螢幕上列印的值。 具體而言，在第一個建立的卡片中，實例識別碼是 *ROOT\SMARTCARDREADER\0000*。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
