---
title: 在命名空間上啟用存取型列舉
description: 本文說明如何在命名空間上啟用存取型列舉。
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 04b023a931f8d66205a07f05bb8d3e955f8b83ca
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964070"
---
# <a name="enable-access-based-enumeration-on-a-namespace"></a>在命名空間上啟用存取型列舉

> 適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

存取型列舉可以隱藏使用者沒有存取權限的檔案和資料夾。 預設情況下不會為 DFS 命名空間啟用此功能。 您可以使用 DFS 管理來啟用 DFS 資料夾的存取型列舉。 若要控制資料夾目標中檔案和資料夾的存取型列舉，您必須使用 \[共用與存放管理\]，在每個共用資料夾上啟用存取型列舉。

若要在命名空間上啟用存取型列舉，所有命名空間伺服器都必須執行 Windows Server 2008 或更新版本。 此外，網域型命名空間也必須使用 Windows Server 2008 模式。 如需 Windows Server 2008 模式的需求相關資訊，請參閱[選擇命名空間類型](choose-a-namespace-type.md)。

在某些環境中，啟用存取型列舉可能會造成伺服器上 CPU 使用率偏高並減緩使用者的回應時間。

> [!NOTE]
> 如果您將網域功能升級到 Windows Server 2008 時仍有現有的網域型命名空間，DFS 管理可讓您在這些命名空間上啟用存取型列舉。 不過，您將無法編輯權限來向任何群組或使用者隱藏資料夾，除非您將命名空間移轉至 Windows Server 2008 模式。 如需詳細資訊，請參閱[將網域型命名空間移轉到 Windows Server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)。


若要在 DFS 命名空間上使用存取型列舉，您必須執行下列步驟：

-   在命名空間上啟用存取型列舉
-   控制哪些使用者和群組可以檢視個別 DFS 資料夾


> [!WARNING]
> 存取型列舉無法防止使用者轉介至他們已知其 DFS 路徑的資料夾目標。 只有資料夾目標 (共用資料夾) 本身的共用權限或 NTFS 檔案系統權限才能防止使用者存取資料夾目標。 DFS 資料夾權限僅能用來顯示或隱藏 DFS 資料夾，不能用於在 DFS 資料夾層級控制存取、設定相關權限的讀取存取權。 如需詳細資訊，請參閱[使用繼承的權限搭配存取型列舉](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd834874(v=ws.11))

<br />
您可以使用 Windows 介面或使用命令列，來啟用命名空間的存取型列舉。

## <a name="to-enable-access-based-enumeration-by-using-the-windows-interface"></a>若要使用 Windows 介面啟用存取型列舉

1.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，於適當的命名空間上按一下滑鼠右鍵，再按一下 **\[內容\]**。

2.  按一下 **\[進階\]** 索引標籤，然後選取 **\[啟用此命名空間的存取型列舉\]** 核取方塊。

## <a name="to-enable-access-based-enumeration-by-using-a-command-line"></a>若要使用命令列啟用存取型列舉

1.  在已安裝 [**分散式檔案系統**角色服務] 或 [**分散式檔案系統工具**] 功能的伺服器上，開啟 [命令提示字元] 視窗。

2.  輸入下列命令，其中 *<命名空間 \_ 根>* 是命名空間的根目錄：

    ```
    dfsutil property abe enable \\ <namespace_root>
    ```

> [!TIP]
> 若要使用 Windows PowerShell 來管理命名空間上的存取型列舉，請使用 [Set-DfsnRoot](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd834874(v=ws.11))、[Grant-DfsnAccess](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd834874(v=ws.11)) 和 [Revoke-DfsnAccess](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd834874(v=ws.11)) Cmdlet。 DFSN Windows PowerShell 模組於 Windows Server 2012 中引進。

您可以使用 Windows 介面或使用命令列，來控制哪些使用者和群組可以檢視哪些 DFS 資料夾。

## <a name="to-control-folder-visibility-by-using-the-windows-interface"></a>若要使用 Windows 介面控制資料夾可見性

1.  在主控台的 **\[命名空間\]** 節點下方，找到您要控制可見性的含目標資料夾，以滑鼠右鍵按一下它，然後按一下 **\[屬性\]**。

2.  按一下 [進階] 索引標籤。

3.  按一下 **\[設定 DFS 資料夾的明確檢視權限]**，然後按一下 **\[設定檢視權限\]**。

4.  按一下 **\[新增\]** 或 **\[移除\]**，新增或移除群組或使用者。

5.  若要允許使用者查看 DFS 資料夾，請選取群組或使用者，然後選取 **\[允許\]** 核取方塊。

    若要隱藏資料夾不顯示給群組或使用者，請選取群組或使用者，然後選取 **\[拒絕\]** 核取方塊。

## <a name="to-control-folder-visibility-by-using-a-command-line"></a>若要使用命令列控制資料夾可見性

1. 在具有 **\[分散式檔案系統\]** 角色服務或已安裝 **\[分散式檔案系統工具\]** 功能的伺服器上，開啟命令提示字元視窗。

2. 輸入下列命令，其中* &lt; DFSPath &gt; *是 DFS 資料夾（連結）的路徑， *<網域 \\ 帳戶>* 是群組或使用者帳戶的名稱，而 *（...）* 會取代為其他存取控制專案（ace）：

   ```
   dfsutil property sd grant <DFSPath> DOMAIN\Account:R (...) Protect Replace
   ```

   例如，若要以允許 Domain Admins 和 CONTOSO \\ 講師群組讀取（R）存取 office\public\training 資料夾的許可權來取代現有許可權 \\ ，請輸入下列命令：

   ```
   dfsutil property sd grant \\contoso.office\public\training "CONTOSO\Domain Admins":R CONTOSO\Trainers:R Protect Replace
   ```

3. 若要從命令提示字元執行其他工作，請使用下列命令：


| 命令 | 描述 |
|---|---|
|[Dfsutil property sd deny](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759150(v=ws.11))|拒絕群組或使用者檢視資料夾的能力。|
|[Dfsutil property sd reset](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759150(v=ws.11)) |移除資料夾的所有權限。|
|[Dfsutil property sd revoke](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759150(v=ws.11))| 從資料夾移除群組或使用者 ACE。 |

## <a name="additional-references"></a>其他參考

-   [建立 DFS 命名空間](create-a-dfs-namespace.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)
-   [安裝 DFS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731089(v=ws.11))
-   [使用繼承的許可權搭配以存取為基礎的列舉](using-inherited-permissions-with-access-based-enumeration.md)
