---
title: icacls
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 494c87073cfd78c7f5e17c72d4c65bec33a49b98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375495"
---
# <a name="icacls"></a>icacls

顯示或修改指定檔案上的判別存取控制清單 (DACL)，及套用預存的 DACL 到指定目錄中的檔案。

如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
icacls <FileName> [/grant[:r] <Sid>:<Perm>[...]] [/deny <Sid>:<Perm>[...]] [/remove[:g|:d]] <Sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<Policy>[...]]
icacls <Directory> [/substitute <SidOld> <SidNew> [...]] [/restore <ACLfile> [/c] [/l] [/q]]
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|\<FileName >|指定要顯示其 Dacl 的檔案。|
|\<目錄 >|指定要顯示其 Dacl 的目錄。|
|一起|對目前目錄及其子目錄中的所有指定檔案執行作業。|
|/c|即使有任何檔案錯誤，仍繼續操作。 仍然會顯示錯誤訊息。|
|/l|在符號連結和其目的地上執行運算。|
|/q|隱藏成功訊息。|
|[/save \<ACLfile > [/t] [/c] [/l] [/q]]|將所有相符檔案的 Dacl 儲存至*ACLfile* ，以供稍後用於 **/restore**。|
|[/setowner \<Username > [/t] [/c] [/l] [/q]]|將所有相符檔案的擁有者變更為指定的使用者。|
|[/findSID \<Sid > [/t] [/c] [/l] [/q]]|尋找包含 DACL 的所有相符檔案，明確提及指定的安全識別碼（SID）。|
|[/verify [/t] [/c] [/l] [/q]]|尋找 Acl 不是標準的所有檔案，或長度與 ACE 不一致的檔案（存取控制專案）計數。|
|[/reset [/t] [/c] [/l] [/q]]|以預設繼承的 Acl 取代所有相符檔案的 Acl。|
|[/grant [： r] \<Sid >：<Perm>[...]]|授與指定的使用者存取權限。 許可權會取代先前授與的明確許可權。</br>如果沒有 **： r**，許可權就會新增至任何先前授與的明確許可權。|
|[/deny \<Sid >：<Perm>[...]]|明確拒絕指定的使用者存取權限。 系統會為指定的許可權新增明確的拒絕 ACE，並移除任何明確授與中的相同許可權。|
|[/remove [： g\|:d]] \<Sid > [...]]一起/c/l一起|從 DACL 中移除所有出現的指定 SID。</br>**： g**會移除所指定 SID 的所有已授與許可權。</br>**:d**移除指定之 SID 的所有已拒絕許可權。|
|[/setintegritylevel [（CI）（OI）]\<層級 >：<Policy>[...]]|將完整性 ACE 明確新增至所有相符的檔案。 *層級*指定為：</br>-   **L**[允許]</br>-   **M**[edium]</br>-   **H**[igh]</br>完整性 ACE 的繼承選項可能在層級之前，而且僅適用于目錄。|
|[/substitute \<SidOld > <SidNew> [...]]|以新的 SID （*SidNew*）取代現有的 Sid （*SidOld*）。 需要*Directory*參數。|
|/restore \<ACLfile > [/c] [/l] [/q]|將預存的 Dacl 從*ACLfile*套用至指定目錄中的檔案。 需要*Directory*參數。|
|/inheritancelevel： [e\|d\|r]|設定繼承層級： <br>  **e** -啟用 enheritance <br>**d** -停用繼承並複製 ace <br>**r** -移除所有繼承的 ace

## <a name="remarks"></a>備註

-   Sid 可以是數位或易記名稱格式。 如果您使用數值形式，請在 SID 開頭貼 **&#42;** 上萬用字元。
-   **icacls**會保留 ACE 專案的標準順序，如下所示：  
    -   明確拒絕
    -   明確授與
    -   繼承拒絕
    -   繼承的授與
-   *永久*是可使用下列其中一種形式指定的許可權遮罩：  
    -   一系列的簡單許可權：

        **F** （完整存取）

        **M** （修改存取權）

        **RX** （讀取及執行存取權）

        **R** （唯讀存取）

        **W** （僅限寫入存取）
    -   以括弧括住的特定許可權清單（以逗號分隔）：

        **D** （delete）

        **RC** （讀取控制）

        **WDAC** （寫入 DAC）

        **WO** （寫入擁有者）

        **S** （同步處理）

        **AS** （存取系統安全性）

        **MA** （允許的最大值）

        **GR** （一般讀取）

        **GW** （一般寫入）

        **GE** （一般執行）

        **GA** （全部為一般）

        **RD** （讀取資料/清單目錄）

        **WD** （寫入資料/新增檔案）

        **AD** （附加資料/新增子目錄）

        **反應**（讀取擴充屬性）

        **WEA** （寫入擴充屬性）

        **X** （執行/遍歷）

        **DC** （刪除子系）

        **RA** （讀取屬性）

        **WA** （寫入屬性）
-   繼承許可權可能會在*永久*表單的前面，而且僅適用于目錄：

    **（OI）** ：物件繼承

    **（CI）** ：容器繼承

    **（IO）** ：只繼承

    **（NP）** ：不要傳播繼承

## <a name="examples"></a>範例

若要將 C：\Windows 目錄及其子目錄中所有檔案的 Dacl 儲存至 ACLFile 檔案，請輸入：

```
icacls c:\windows\* /save aclfile /t
```

若要還原 ACLFile 中存在於 C：\Windows 目錄及其子目錄中的每個檔案的 Dacl，請輸入：

```
icacls c:\windows\ /restore aclfile
```

若要對名為 "Test1" 的檔案授與使用者 User1 Delete 和 Write DAC 許可權，請輸入：

```
icacls test1 /grant User1:(d,wdac)
```

若要將 SID S-1-1-0 Delete 和 Write DAC 許可權所定義的使用者授與名為 "Test2" 的檔案，請輸入：

```
icacls test2 /grant *S-1-1-0:(d,wdac)
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
