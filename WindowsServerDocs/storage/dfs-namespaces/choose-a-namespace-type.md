---
title: 選擇命名空間類型
description: 本文說明如何選擇命名空間類型。
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: becaab1b4c35492200ad3d75d5f31829d85e10f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402193"
---
# <a name="choose-a-namespace-type"></a>選擇命名空間類型

> 適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

建立命名空間時，您必須選擇兩種命名空間其中一種：獨立命名空間或網域型命名空間。 此外，如果您選擇網域型命名空間，則必須選擇命名空間模式：Windows 2000 Server 模式或 Windows Server 2008 模式。

## <a name="choosing-a-namespace-type"></a>選擇命名空間類型

如果您的環境符合下列任何條件，請選擇獨立命名空間：

-   您的組織未使用 Active Directory Domain Services （AD DS）。
-   您想使用容錯移轉叢集來提高命名空間可用性。
-   您必須在不符合網域型命名空間（Windows Server 2008 模式）需求的網域中，建立具有超過5000個 DFS 資料夾的單一命名空間，如本主題稍後所述。

> [!NOTE]
> 若要檢查命名空間的大小，請以滑鼠右鍵按一下 DFS 管理主控台樹狀目錄中的命名空間、按一下 **\[屬性\]** ，然後檢視 **\[命名空間屬性\]** 對話方塊中的命名空間大小。 如需 DFS 命名空間延展性的詳細資訊，請參閱 Microsoft 網站[檔案服務](https://technet.microsoft.com/library/cc771548.aspx)。

如果您的環境符合下列任何條件，請選擇網域型命名空間：

-   您想使用多個命名空間伺服器來確保命名空間的可用性。
-   您想隱藏命名空間伺服器的名稱，不顯示給使用者。 如此可輕鬆更換命名空間伺服器或將命名空間移轉至其他伺服器。

## <a name="choosing-a-domain-based-namespace-mode"></a>選擇網域型命名空間模式

如果您選擇網域型命名空間，則必須選擇是要使用 Windows 2000 伺服器模式還是 Windows Server 2008 模式。 Windows Server 2008 模式包含存取型列舉和增加擴充性的支援。 Windows 2000 Server 中引進的網域型命名空間現在稱為「網域型命名空間（Windows 2000 伺服器模式）」。

若要使用 Windows Server 2008 模式，網域和命名空間必須符合下列最低需求：

-   樹系使用 Windows Server 2003 或更高的樹系功能等級。
-   網域會使用 Windows Server 2008 或更高版本的網域功能等級。
-   所有命名空間伺服器都執行 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008。

如果您的環境支援，請在建立以網域為基礎的新命名空間時選擇 Windows Server 2008 模式。 此模式會提供額外的功能和擴充性，而且也不可能需要從 Windows 2000 伺服器模式遷移命名空間。

如需將命名空間遷移至 Windows Server 2008 模式的相關資訊，請參閱將[網域型命名空間遷移至 Windows server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)。

如果您的環境不支援 Windows Server 2008 模式中的網域型命名空間，請使用現有的 Windows 2000 伺服器模式做為命名空間。

## <a name="comparing-namespace-types-and-modes"></a>比較命名空間類型與模式

下表描述每個命名空間類型與模式的特性。

|特性|獨立命名空間|網域型命名空間 (Windows 2000 Server 模式) |網域型命名空間 (Windows Server 2008 模式) | 
|---|---|---|---|
|命名空間的路徑|\\ @ no__t-1*ServerName\RootName* |\\ @ no__t-1*NetBIOSDomainName\RootName* <br />\\ @ no__t-1*DNSDomainName\RootName*|\\ @ no__t-1*NetBIOSDomainName\RootName* <br /> \\ @ no__t-1*DNSDomainName\RootName*|
|命名空間資訊儲存位置|在登錄以及命名空間伺服器的記憶體快取中|在 AD DS 以及每個命名空間伺服器的記憶體快取中|在 AD DS 以及每個命名空間伺服器的記憶體快取中|
|命名空間大小建議|命名空間可以包含超過 5000 個含目標的資料夾。建議的限制是 50000 個含目標的資料夾|AD DS 中命名空間物件的大小應該低於 5 MB，以維持與未執行 Windows Server 2008 之網域控制站的相容性。 這表示不超過大約 5000 個含目標的資料夾。|命名空間可以包含超過 5000 個含目標的資料夾。建議的限制是 50000 個含目標的資料夾 |
|最低 AD DS 樹系功能等級|不需要 AD DS|Windows 2000|Windows Server 2003|
|最低 AD DS 網域功能等級|不需要 AD DS|Windows 2000 混合模式|Windows Server 2008|
|最低支援的命名空間伺服器|Windows 2000 Server|Windows 2000 Server|Windows Server 2008|
|支援存取型列舉 (若啟用)|是，需要 Windows Server 2008 命名空間伺服器|否|是|
|確保命名空間可用性的支援方法|在容錯移轉叢集上建立獨立命名空間。|使用多個命名空間伺服器來裝載命名空間。 (命名空間伺服器必須位於相同的網域中)。|使用多個命名空間伺服器來裝載命名空間。 (命名空間伺服器必須位於相同的網域中)。|
|支援使用 DFS 複寫來複寫資料夾目標|加入 AD DS 網域時支援|支援|支援|

## <a name="see-also"></a>另請參閱

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [將網域型命名空間移轉到 Windows Server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)


