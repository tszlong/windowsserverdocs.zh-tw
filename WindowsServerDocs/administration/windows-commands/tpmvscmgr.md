---
title: tpmvscmgr
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6cfc15b7c089c9b6ad4d9a267b951373e63a63a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888589"
---
# <a name="tpmvscmgr"></a>tpmvscmgr



Tpmvscmgr 命令列工具可讓使用者以系統管理認證來建立和刪除 TPM 虛擬智慧卡的電腦上。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
Tpmvscmgr create [/name] [/AdminKey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```
```
Tpmvscmgr destroy [/instance <instance ID>] [/?]
```

### <a name="parameters-for-create-command"></a>建立命令的參數

Create 命令會設定新使用者的系統上的虛擬智慧卡。 需要刪除時，它會傳回新建立的卡片，供日後參考的執行個體識別碼。 執行個體識別碼的格式**ROOT\SMARTCARDREADER\000n**何處**n**從 0 開始，並會加上 1 每次當您建立新的虛擬智慧卡。

|參數|描述|
|---------|-----------|
|/name|必要。 指出新的虛擬智慧卡的名稱。|
|/AdminKey|表示可用來重設智慧卡 PIN，如果使用者忘記 pin 碼的所需的系統管理員金鑰。</br>**預設**指定 010203040506070801020304050607080102030405060708 的預設值。</br>**提示**會提示使用者輸入系統管理員索引鍵的值。</br>**隨機**導致卡片，不會傳回給使用者的系統管理員索引鍵的隨機設定。 這會建立卡片，可能會無法管理使用智慧卡管理工具。 當使用隨機產生，系統管理員索引鍵必須輸入為 48 個十六進位字元。|
|/PIN|表示所需的使用者 PIN 值。</br>**預設**指定預設的 12345678 的 pin 碼。</br>**提示**會提示使用者輸入 PIN，以在命令列。 PIN 必須是至少八個字元，而且它可以包含數字、 字元和特殊字元。|
|/PUK|指出所需的 PIN 解除鎖定索引鍵 (PUK) 值。 PUK 值必須是至少八個字元，而且它可以包含數字、 字元和特殊字元。 如果省略此參數，卡片會建立沒有 PUK。</br>**預設**指定預設的 12345678 PUK。</br>**提示**會提示使用者輸入 PUK 在命令列。|
|/generate|虛擬智慧卡的函式需要的儲存體中，會產生檔案。 如果產生 / 省略參數時，它就相當於建立卡片，而不需要此檔案系統。 無檔案系統的卡片只受 Microsoft Configuration Manager 之類的智慧卡管理系統。|
|/machine|可讓您指定遠端電腦，可以建立虛擬智慧卡的名稱。 這可用在網域環境中，而且它仰賴 DCOM。 命令中成功建立虛擬智慧卡不同的電腦上，執行此命令的使用者必須是遠端電腦上本機 administrators 群組中的成員。|
|/?|此命令，會顯示說明。|

### <a name="parameters-for-destroy-command"></a>Destroy 命令的參數

[終結] 命令會從使用者的電腦，安全地刪除虛擬智慧卡。

> [!WARNING]
> 刪除虛擬智慧卡時，就無法復原。

|參數|描述|
|---------|-----------|
|/instance|指定要移除虛擬智慧卡的執行個體識別碼。 InstanceID 產生做為輸出的 Tpmvscmgr.exe 卡片建立時。 /Instance 參數是必要的欄位，如 [終結] 命令。|
|/?|此命令，會顯示說明。|

## <a name="remarks"></a>備註

中的成員資格**系統管理員**群組 （或同等權限） 在目標電腦上的最小，才能執行此命令的所有參數。

若為英數字元的輸入，允許完整 127 的 ASCII 字元集。

## <a name="BKMK_Examples"></a>範例

下列命令示範如何建立虛擬智慧卡，稍後可將從另一部電腦啟動的智慧卡管理工具。
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey DEFAULT /PIN PROMPT
```
或者，而不是使用預設的系統管理員金鑰，您可以建立系統管理員金鑰在命令列。 下列命令示範如何建立非系統管理員金鑰。
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey PROMPT /PIN PROMPT
```
下列命令會建立未受管理的虛擬智慧卡，可以用來註冊憑證。
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey RANDOM /PIN PROMPT /generate
```
下列命令會建立虛擬智慧卡使用隨機的系統管理員金鑰。 建立 cardis 之後自動捨棄索引鍵。 這表示，如果使用者忘記 PIN，或想要變更 PIN，使用者必須刪除卡片，然後重新建立它。 若要刪除卡片，使用者可以執行下列命令。
```
tpmvscmgr.exe destroy /instance <instance ID> 
```
其中\<執行個體識別碼 > 值列印在螢幕上時建立卡片的使用者。 具體來說，建立第一個卡片的情況下，執行個體識別碼是 ROOT\SMARTCARDREADER\0000。

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)