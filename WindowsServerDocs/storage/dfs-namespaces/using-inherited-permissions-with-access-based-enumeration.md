---
title: 使用繼承的權限搭配存取型列舉
description: 本文說明如何使用繼承的權限搭配存取型列舉
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 112ec4363177ac6dd560493843c8937bdfbac4de
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475145"
---
# <a name="using-inherited-permissions-with-access-based-enumeration"></a>使用繼承的權限搭配存取型列舉

> 適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

根據預設，用於 DFS 資料夾的權限是繼承自命名空間伺服器的本機檔案系統。 許可權繼承自系統磁片磁碟機的根目錄，並將讀取權限授與網域 \\ 使用者群組。 因此，就算啟用存取型列舉，命名空間中的所有資料夾仍維持顯示給所有網域使用者。

## <a name="advantages-and-limitations-of-inherited-permissions"></a>繼承的權限的優點和限制

使用繼承的權限來控制哪些使用者可以檢視 DFS 命名空間的資料夾有兩個主要優點：

-   您可以將繼承的權限快速套用至多個資料夾，而不必使用指令碼。
-   您可以將繼承的權限套用至命名空間根目錄和不含目標的資料夾。

儘管具有以上優點，DFS 命名空間中的繼承權限仍有許多限制，因此不適用於大多數環境：

-   對於繼承的權限所進行的修改不會複寫至其他命名空間伺服器。 因此，繼承的權限僅適用於獨立命名空間，或是您可以實作第三方複寫系統以在所有命名空間伺服器上保持存取控制清單 (ACL) 同步的環境。
-   DFS 管理和 **Dfsutil** 無法檢視或修改繼承的權限。 因此，您必須使用 Windows 檔案總管或 **Icacls** 命令，再加上 DFS 管理命令或 **Dfsutil** 來管理命名空間。
-   使用繼承的權限時，除非使用 **Dfsutil** 命令，否則無法修改含目標資料夾的權限。 DFS 命名空間會自動移除使用其他工具或方法來設定的含目標資料的權限。
-   如果您在使用繼承的權限時設定含目標資料的權限，您在含目標資料上設定的 ACL 會與繼承自檔案系統中父系資料夾的權限合併。 您必須檢查這兩組權限，判斷淨權限是什麼。

> [!NOTE]
> 使用繼承的權限時，最簡單的方法是在命名空間根目錄和不含目標的資料夾上設定權限。 接著在含目標的資料夾上使用繼承的權限，使其繼承父系的所有權限。

## <a name="using-inherited-permissions"></a>使用繼承的權限

若要限制哪些使用者可以檢視 DFS 資料夾，您必須執行下列其中一項工作：

-   **設定明確的資料夾權限，停用繼承。** 若要使用 DFS 管理或 **Dfsutil** 命令在含目標的資料夾 (連結) 上設定明確的權限，請參閱[在命名空間上啟用存取型列舉](enable-access-based-enumeration-on-a-namespace.md)。
-   **修改本機檔案系統父系上的繼承的權限**。 若要修改繼承自含目標資料夾的權限，如果您已經在資料夾上設定明確的權限，請從明確的權限切換至繼承的權限，如下所述。 接著使用 Windows 檔案總管或 **Icacls** 命令修改資料夾 (含目標的資料夾會繼承其權限) 的權限。

> [!NOTE]
> 存取型列舉無法防止使用者轉介至他們已知含目標資料夾之 DFS 路徑的資料夾目標。 使用 Windows 檔案總管或 **Icacls** 命令在命名空間根目錄或不含目標資料夾上設定的權限集，控制著使用者是否可以存取 DFS 資料夾或命名空間根目錄。 不過，使用此方法無法防止使用者直接存取含目標的資料夾。 只有共用資料夾本身的共用權限或 NTFS 檔案系統權限，才能防止使用者存取資料夾目標。

## <a name="to-switch-from-explicit-permissions-to-inherited-permissions"></a>若要從明確的權限切換至繼承的權限

1.  在主控台的 **\[命名空間\]** 節點下方，找到您要控制其可見性的含目標資料夾，以滑鼠右鍵按一下該資料夾，然後按一下 **\[屬性\]**。

2.  按一下 [**進階**] 索引標籤。

3.  按一下 **\[使用從本機檔案系統繼承的權限\]**，然後按一下 **\[確認使用繼承的權限\]** 對話方塊中的 **\[確定\]**。 這樣會移除此資料夾上所有明確設定的權限、還原命名空間伺服器的本機檔案系統的繼承 NTFS 權限。

4.  若要變更 DFS 命名空間中資料夾或命名空間根目錄的繼承權限，請使用 Windows 檔案總管或 **ICacls** 命令。

## <a name="additional-references"></a>其他參考

-   [建立 DFS 命名空間](create-a-dfs-namespace.md)