---
title: icacls
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 403edfcc-328a-479d-b641-80c290ccf73e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 20b2150b1135467cce43ae23bfdc275a5da22141
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852639"
---
# <a name="icacls"></a>icacls



顯示或修改指定檔案上的判別存取控制清單 (DACL)，及套用預存的 DACL 到指定目錄中的檔案。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
icacls <FileName> [/grant[:r] <Sid>:<Perm>[...]] [/deny <Sid>:<Perm>[...]] [/remove[:g|:d]] <Sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<Policy>[...]]
icacls <Directory> [/substitute <SidOld> <SidNew> [...]] [/restore <ACLfile> [/c] [/l] [/q]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<FileName>|指定要顯示 Dacl 的檔案。|
|\<目錄 >|指定要顯示 Dacl 的目錄。|
|/t|執行目前目錄及其子目錄中的所有指定的檔案上執行的作業。|
|/c|繼續作業，而不理會任何檔案錯誤。 仍會顯示錯誤訊息。|
|/l|執行與它的目的端的符號連結上的作業。|
|/q|隱藏成功訊息。|
|[/ 儲存\<ACLfile > [/t] [/c] [/l] [/q]]|儲存到的所有相符檔案的 Dacl *ACLfile*更新版本搭配 **/還原**。|
|[/ 出現了 setowner\<使用者名稱 > [/t] [/c] [/l] [/q]]|變更指定之使用者的所有相符檔案的擁有者。|
|[/findSID \<Sid> [/t] [/c] [/l] [/q]]|尋找包含 DACL，明確提及指定的安全性識別碼 (SID) 的所有相符檔案。|
|[/verify [/t] [/c] [/l] [/q]]|尋找所有的檔案不是標準或具有長度與 ACE （存取控制項目） 的計數不一致的 Acl。|
|[/reset [/t] [/c] [/l] [/q]]|預設值的取代 Acl 繼承的所有相符檔案的 Acl。|
|[/ 授與 [: r] \<Sid >:<Perm>[...]]|授與指定的使用者存取權限。 權限會取代先前授與明確的權限。</br>不含 **: r**，權限會新增至任何先前授與明確的權限。|
|[y \<Sid >:<Perm>[...]]|明確拒絕指定的使用者存取權限。 明確拒絕 ACE 會針對指定的權限加入和移除任何明確授與在相同的權限。|
|[移除 [: g\|: d]] \<Sid > [...]][/t][/c][/l][/q]|移除 DACL 中的所有出現之指定的 SID。</br>**: g**移除指定的 SID 授與權限的所有項目。</br>**: d**移除指定的 SID 的拒絕權限的所有項目。|
|[/ [(CI)(OI)] setintegritylevel\<層級 >:<Policy>[...]]|明確地將所有相符的檔案完整性的 ACE。 *層級*指定為：</br>-   **L**[ow]</br>-   **M**[edium]</br>-   **H**[igh]</br>完整性 ACE 的繼承選項可能會在層級，並只會套用至目錄。|
|[/substitute \<SidOld> <SidNew> [...]]|取代現有的 SID (*SidOld*) 與新的 SID (*SidNew*)。 需要*Directory*參數。|
|還原/\<ACLfile > [/c] [/l] [/q]|適用於從預存的 Dacl *ACLfile*到指定的目錄中的檔案。 需要*Directory*參數。|
|/inheritancelevel:[e\|d\|r]|設定繼承層級： <br>  **e** -可讓 enheritance <br>**d** -停用繼承，並將複製的 Ace <br>**r** -移除所有繼承 Ace

## <a name="remarks"></a>備註

-   Sid 可能在任一數字或易記名稱格式。 如果您使用多種格式，詞綴萬用字元 **&#42;** SID 的開頭。
-   **icacls**保留 ACE 項目，做為標準順序：  
    -   明確的拒絕
    -   明確授與
    -   繼承的拒絕
    -   繼承授與
-   *為永久*是權限遮罩，可以指定其中一種下列形式：  
    -   一系列簡單的權限：

        **F** （完整存取權）

        **M** （修改存取權）

        **RX** （讀取及執行存取權）

        **R** （唯讀存取）

        **W** （唯寫存取權）
    -   以逗號分隔清單的特定權限的括號中：

        **D** （刪除）

        **RC** （讀取控制項）

        **WDAC** （寫入 DAC）

        **WO** （寫入擁有者）

        **S** （同步處理）

        **AS** （存取系統安全性）

        **MA** （最多允許）

        **GR** （一般讀取）

        **GW** （一般寫入）

        **GE** （一般執行）

        **GA** （所有泛用）

        **RD** （讀取/列出的資料目錄）

        **WD** （寫入資料/新增檔案）

        **AD** （附加資料/新增子目錄）

        **反應**（讀取擴充的屬性）

        **WEA** （寫入擴充的屬性）

        **X** （執行/周遊）

        **DC** （刪除子系）

        **RA** （讀取屬性）

        **WA** （寫入屬性）
-   繼承的權限可能會在其中一個*為永久*表單中，而且它們只會套用至目錄：

    **(OI)**： 物件繼承

    **(CI)**： 容器繼承

    **(IO)**： 只有繼承

    **(NP)**： 不會傳播繼承

## <a name="BKMK_examples"></a>範例

若要儲存所有檔案的 Dacl C:\Windows 目錄及其子目錄 ACLFile 檔案中，請輸入：
```
icacls c:\windows\* /save aclfile /t
```
若要還原的每一個檔案內存在 C:\Windows 目錄及其子目錄中的 ACLFile Dacl，請輸入：
```
icacls c:\windows\ /restore aclfile
```
若要授與使用者 User1 刪除，並撰寫 DAC 至名為"Test1"檔案的權限，請輸入：
```
icacls test1 /grant User1:(d,wdac)
```
若要授與 SID S-1-1-0 刪除及撰寫 DAC 權限到檔案，名為"Test2 」，所定義的使用者輸入：
```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
