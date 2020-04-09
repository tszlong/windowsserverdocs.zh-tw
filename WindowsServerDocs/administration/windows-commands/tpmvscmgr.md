---
title: tpmvscmgr
description: 適用于 tpmvscmgr 的 Windows 命令主題，這是一種命令列工具，可讓具有系統管理認證的使用者建立和刪除電腦上的 TPM 虛擬智慧卡。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4411e0ec3c75cd768b2fe32ad26b17331328e3ca
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832731"
---
# <a name="tpmvscmgr"></a>tpmvscmgr

Tpmvscmgr 命令列工具可讓具有系統管理認證的使用者建立和刪除電腦上的 TPM 虛擬智慧卡。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Tpmvscmgr create [/name] [/AdminKey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```
```
Tpmvscmgr destroy [/instance <instance ID>] [/?]
```

#### <a name="parameters-for-create-command"></a>Create 命令的參數

Create 命令會在使用者的系統上設定新的虛擬智慧卡。 如果需要刪除，它會傳回新建立之卡片的實例識別碼，以供日後參考。 實例識別碼的格式為**ROOT\SMARTCARDREADER\000n** ，其中**n**是從0開始，每次建立新的虛擬智慧卡時，都會增加1。

|參數|描述|
|---------|-----------|
|/name|必要。 表示新虛擬智慧卡的名稱。|
|/AdminKey|指出當使用者忘記 PIN 時，可以用來重設卡片 PIN 的所需系統管理員金鑰。</br>**預設值**指定010203040506070801020304050607080102030405060708的預設值。</br>**提示**提示使用者輸入系統管理員金鑰的值。</br>**隨機**導致卡片的系統管理員金鑰隨機設定不會傳回給使用者。 這會建立可能無法使用智慧卡管理工具來管理的卡片。 以隨機產生時，系統管理員金鑰必須輸入為48的十六進位字元。|
|/PIN|表示所需的使用者 PIN 值。</br>**預設值**指定預設的 PIN 碼12345678。</br>**提示**提示使用者在命令列中輸入 PIN。 PIN 必須至少有八個字元，而且可以包含數位、字元和特殊字元。|
|/PUK|指出所需的 PIN 解除鎖定金鑰（PUK）值。 PUK 值至少必須為八個字元，而且可以包含數位、字元和特殊字元。 如果省略此參數，則會建立不含 PUK 的卡片。</br>**預設值**指定12345678的預設 PUK。</br>**提示**提示使用者在命令列中輸入 PUK。|
|/generate|會在儲存體中產生虛擬智慧卡運作所需的檔案。 如果省略/generate 參數，它就相當於建立不含此檔案系統的卡片。 沒有檔案系統的卡片只能由智慧卡管理系統（例如 Microsoft Configuration Manager）進行管理。|
|/machine|可讓您指定可在其上建立虛擬智慧卡的遠端電腦名稱稱。 這只能在網域環境中使用，而且它依賴 DCOM。 執行此命令的使用者必須是遠端電腦上本機系統管理員群組中的成員，才能讓命令成功建立另一部電腦上的虛擬智慧卡。|
|/?|顯示此命令的說明。|

#### <a name="parameters-for-destroy-command"></a>終結命令的參數

[摧毀] 命令會從使用者的電腦安全性地刪除虛擬智慧卡。

> [!WARNING]
> 當虛擬智慧卡被刪除時，就無法復原。

|參數|描述|
|---------|-----------|
|/instance|指定要移除之虛擬智慧卡的實例識別碼。 InstanceID 是在建立卡片時，以 Tpmvscmgr 輸出的形式產生。 /Instance 參數是摧毀命令的必要欄位。|
|/?|顯示此命令的說明。|

## <a name="remarks"></a>備註

若要執行此命令的所有參數，至少需要目的電腦上**Administrators**群組的成員資格（或同等許可權）。

針對英數位元輸入，允許使用完整127字元的 ASCII 集。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

下列命令示範如何建立可在稍後由另一部電腦啟動之智慧卡管理工具管理的虛擬智慧卡。
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey DEFAULT /PIN PROMPT
```
或者，您可以在命令列建立系統管理員金鑰，而不是使用預設的系統管理員金鑰。 下列命令顯示如何建立系統管理員金鑰。
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey PROMPT /PIN PROMPT
```
下列命令會建立可用於註冊憑證的非受控虛擬智慧卡。
```
tpmvscmgr.exe create /name VirtualSmartCardForCorpAccess /AdminKey RANDOM /PIN PROMPT /generate
```
下列命令會建立具有隨機系統管理員金鑰的虛擬智慧卡。 金鑰會在 cardis 建立之後自動捨棄。 這表示，如果使用者忘記 PIN 或想要變更 PIN，使用者必須刪除卡片並重新建立。 若要刪除卡片，使用者可以執行下列命令。
```
tpmvscmgr.exe destroy /instance <instance ID> 
```
其中 \<實例識別碼 > 是使用者建立卡片時，在螢幕上列印的值。 具體而言，針對第一個建立的卡片，實例識別碼為 ROOT\SMARTCARDREADER\0000。

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)