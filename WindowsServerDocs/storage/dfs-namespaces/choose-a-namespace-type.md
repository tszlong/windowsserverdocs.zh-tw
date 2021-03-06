---
title: 選擇命名空間類型
description: 本文說明如何選擇命名空間類型。
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c78e97148dffba920be5e65b19d97594c1b302d1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957676"
---
# <a name="choose-a-namespace-type"></a>選擇命名空間類型

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

建立命名空間時，您必須選擇兩種命名空間其中一種：獨立命名空間或網域型命名空間。 此外，如果選擇網域型命名空間，您必須選擇命名空間模式：Windows 2000 Server 模式或 Windows Server 2008 模式。

## <a name="choosing-a-namespace-type"></a>選擇命名空間類型

如果您的環境符合下列任何條件，請選擇獨立命名空間：

-   您的組織未使用 Active Directory 網域服務 (AD DS)。
-   您想使用容錯移轉叢集來提高命名空間可用性。
-   您必須建立單一命名空間且在網域中包含超過 5000 個 DFS 資料夾，但不符合本主題稍後所述的網域型命名空間 (Windows Server 2008 模式) 需求。

> [!NOTE]
> 若要檢查命名空間的大小，請以滑鼠右鍵按一下 DFS 管理主控台樹狀目錄中的命名空間、按一下 **\[屬性\]**，然後檢視 **\[命名空間屬性\]** 對話方塊中的命名空間大小。 如需 DFS 命名空間延展性的詳細資訊，請參閱 Microsoft 網站[檔案服務](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771548(v=ws.10))。

如果您的環境符合下列任何條件，請選擇網域型命名空間：

-   您想使用多個命名空間伺服器來確保命名空間的可用性。
-   您想隱藏命名空間伺服器的名稱，不顯示給使用者。 如此可輕鬆更換命名空間伺服器或將命名空間移轉至其他伺服器。

## <a name="choosing-a-domain-based-namespace-mode"></a>選擇網域型命名空間模式

如果選擇網域型命名空間，您還必須選擇使用 Windows 2000 Server 模式或 Windows Server 2008 模式。 Windows Server 2008 模式包含支援存取型列舉和提高延展性。 Windows 2000 Server 中引進的網域型命名空間現在稱為「網域型命名空間 (Windows 2000 Server 模式)」。

若要使用 Windows Server 2008 模式，網域和命名空間必須符合下列最低需求：

-   樹系使用 Windows Server 2003 或更高的樹系功能等級。
-   網域使用 Windows Server 2008 或更高的網域功能等級。
-   所有命名空間伺服器執行 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008。

如果您的環境可以支援，在建立新的網域型命名空間時請選擇 Windows Server 2008 模式。 此模式提供其他功能與延展性，也不再需要從 Windows 2000 Server 模式移轉命名空間。

如需將命名空間移轉到 Windows Server 2008 模式的相關資訊，請參閱[將網域型命名空間移轉到 Windows Server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)。

如果您的環境不支援 Windows Server 2008 模式的網域型命名空間，請將現有的 Windows 2000 Server 模式用於命名空間。

## <a name="comparing-namespace-types-and-modes"></a>比較命名空間類型與模式

下表描述每個命名空間類型與模式的特性。

|特性|獨立命名空間|網域型命名空間 (Windows 2000 Server 模式) |網域型命名空間 (Windows Server 2008 模式) |
|---|---|---|---|
|命名空間的路徑|\\\ *ServerName\RootName* |\\\ *NetBIOSDomainName\RootName* <br />\\\ *DNSDomainName\RootName*|\\\ *NetBIOSDomainName\RootName* <br /> \\\ *DNSDomainName\RootName*|
|命名空間資訊儲存位置|在登錄以及命名空間伺服器的記憶體快取中|在 AD DS 以及每個命名空間伺服器的記憶體快取中|在 AD DS 以及每個命名空間伺服器的記憶體快取中|
|命名空間大小建議|命名空間可以包含超過 5000 個含目標的資料夾。建議的限制是 50000 個含目標的資料夾|AD DS 中命名空間物件的大小應該低於 5 MB，以維持與未執行 Windows Server 2008 之網域控制站的相容性。 這表示不超過大約 5000 個含目標的資料夾。|命名空間可以包含超過 5000 個含目標的資料夾。建議的限制是 50000 個含目標的資料夾 |
|最低 AD DS 樹系功能等級|不需要 AD DS|Windows 2000|Windows Server 2003|
|最低 AD DS 網域功能等級|不需要 AD DS|Windows 2000 混合模式|Windows Server 2008|
|最低支援的命名空間伺服器|Windows 2000 Server|Windows 2000 Server|Windows Server 2008|
|支援存取型列舉 (若啟用)|是，需要 Windows Server 2008 命名空間伺服器|否|是|
|確保命名空間可用性的支援方法|在容錯移轉叢集上建立獨立命名空間。|使用多個命名空間伺服器來裝載命名空間。 (命名空間伺服器必須位於相同的網域中)。|使用多個命名空間伺服器來裝載命名空間。 (命名空間伺服器必須位於相同的網域中)。|
|支援使用 DFS 複寫來複寫資料夾目標|加入 AD DS 網域時支援|支援|支援|

## <a name="additional-references"></a>其他參考資料

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [將網域型命名空間移轉到 Windows Server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)
