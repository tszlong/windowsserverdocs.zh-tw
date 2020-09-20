---
title: 開始使用 Windows Admin Center
description: 開始使用 Windows Admin Center
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 02/15/2019
ms.openlocfilehash: e5ea882347a0605d814682b50c2ec71e24047472
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766971"
---
# <a name="get-started-with-windows-admin-center"></a>使用 Windows Admin Center 開始

>適用於：Windows Admin Center、Windows Admin Center 預覽版

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [深入了解 Windows Admin Center](../overview.md) 或[立即下載](../overview.md)。

## <a name="windows-admin-center-installed-on-windows-10"></a>Windows Admin Center 安裝在 Windows 10

> [!IMPORTANT]
> 您必須是本機系統管理員群組的成員，才能在 Windows 10 上使用 Windows Admin Center

### <a name="selecting-a-client-certificate"></a>選取用戶端憑證

當您第一次開啟 Windows 10 上的 Windows Admin Center 時，請務必選取 *Windows Admin Center 用戶端* 憑證 (否則您會收到 HTTP 403 錯誤，指出「無法進入此頁面」 ) 。

在 Microsoft Edge 中，系統會使用此對話方塊提示您：

1. 按一下 [**更多選擇**]

    ![選取已醒目提示更多選擇的憑證方塊](../media/launch-cert-1.png)

2. 選取標示 **Windows Admin Center 用戶端** 的憑證，然後按一下 **[確定]**

    ![選取顯示可用憑證的證書框](../media/launch-cert-2.png)

3. 確定已選取 [**一律允許存取**]，然後按一下 [**允許**]

    !需要認證對話方塊] (。/media/launch-cert-3.png) 

## <a name="connecting-to-managed-nodes-and-clusters"></a>連接到受管理的節點和叢集

當您完成 Windows Admin Center 的安裝之後，您可以從主 [總覽] 頁面新增要管理的伺服器或叢集。

 **將單一伺服器或叢集新增為受控節點**

1. 按一下 [所有連線] 底下的 [+ 新增]。

   ![Windows Admin Center - 所有連線頁面](../media/launch/addserver0.png)

2. 選擇新增伺服器、叢集、Windows 電腦或 Azure VM：

   ![Windows Admin Center-新增資源頁面](../media/launch/ChooseConnectionType.png)

3. 輸入要管理的伺服器或叢集的名稱，然後按一下 [ **提交**]。 伺服器或叢集將會新增至 [總覽] 頁面上的連接清單。

   ![Windows Admin Center-伺服器] 頁面](../media/launch/addserver2.png)

   **--或--**

**大量匯入多部伺服器**

 1. 在 [ **新增伺服器連接** ] 頁面上，選擇 [匯 **入伺服器** ] 索引標籤。

    ![Windows Admin Center-匯入伺服器] 索引標籤](../media/launch/import-servers.png)

 2. 按一下 **[流覽]** 並選取文字檔，其中包含您想要新增之伺服器的 fqdn （以逗號分隔）或新行分隔的 fqdn 清單。

> [!Note]
> [使用 PowerShell 來匯出您的連接](#use-powershell-to-import-or-export-your-connections-with-tags)所建立的 .csv 檔案，會包含伺服器名稱以外的其他資訊，而且與此匯入方法不相容。

  **--或--**

**藉由搜尋 Active Directory 來新增伺服器**

 1. 在 [ **新增伺服器連接** ] 頁面上，選擇 [ **搜尋 Active Directory] 索引** 標籤。

    ![Windows Admin Center-搜尋 Active Directory] 索引標籤](../media/launch/search-ad.png)

 2. 輸入您的搜尋準則，然後按一下 [ **搜尋**]。 支援萬用字元 ( * ) 。

 3. 搜尋完成後，請選取一或多個結果，並選擇性地新增標記，然後按一下 [ **新增**]。

## <a name="authenticate-with-the-managed-node"></a>使用受管理的節點進行驗證 ##

Windows Admin Center 支援數個使用受控節點進行驗證的機制。 預設值為 [單一登入]。

**單一登入**

您可以使用目前的 Windows 認證，向受管理的節點進行驗證。 這是預設值，當您新增伺服器時，Windows Admin Center 會嘗試登入。

**在部署為 Windows Server 上的服務時設定單一登入**

如果您已在 Windows Server 上安裝 Windows Admin Center，則需要進行其他設定才能進行單一登入。  [設定您的環境以進行委派](../configure/user-access-control.md)

**--或--**

**使用 [ *管理* 身分] 來指定認證**

在 [ **所有連接**] 底下，從清單中選取伺服器，然後選擇 [ **管理** 身分]，以指定您將用來對受管理節點進行驗證的認證：

![所有連接，管理為選項](../media/launch-use-6.png)

如果 Windows Admin Center 在 Windows Server 上的服務模式下執行，但您未設定 Kerberos 委派，則必須重新輸入您的 Windows 認證：

![指定認證頁面](../media/launch-use-7.png)

您可以將認證套用至所有連線，以針對該特定瀏覽器會話進行快取。 如果您重載瀏覽器，則必須重新輸入您的 [ **管理** 身分] 認證。

**區域系統管理員密碼解決方案 (LAPS) **

如果您的環境使用 [LAPS](/previous-versions/mt227395(v=msdn.10))，且您的 Windows 10 電腦上已安裝 Windows Admin Center，則可以使用 LAPS 認證來向受管理的節點進行驗證。 **如果您使用此案例，請**[提供意見](https://aka.ms/WACFeedback)反應。

## <a name="using-tags-to-organize-your-connections"></a>使用標記來組織您的連接

您可以使用標記來識別及篩選連接清單中的相關伺服器。  這可讓您在 [連接] 清單中看到伺服器的子集。  如果您有許多連接，這會特別有用。

### <a name="edit-tags"></a>編輯標記

* 在 [所有連接] 清單中選取伺服器或多部伺服器
* 在 [**所有連接**] 底下，按一下 [**編輯標記**]。

![Windows Admin Center-編輯標記選項](../media/launch/tags-5.png)

[ **編輯連接標記** ] 窗格可讓您修改、新增或移除所選連線 (的標記) ：

* 若要將新標籤新增至選取的連接 (s) ，請選取 [ **新增** 標籤]，然後輸入您想要使用的標籤名稱。

* 若要使用現有的標記名稱來標記選取的連接，請核取您想要套用之標記名稱旁邊的方塊。

* 若要從所有選取的連接中移除標記，請取消核取您想要移除之標記旁的方塊。

* 如果將標記套用至選取的連接子集，則會以中繼狀態顯示該核取方塊。 您可以按一下方塊來檢查它，並將標籤套用至所有選取的連接，或再按一下以取消核取該標籤，並從所有選取的連接中移除該標記。

![Windows Admin Center-編輯連接標記頁面](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>依標記篩選連接

一旦將標記新增至一或多個伺服器連接之後，您就可以在連接清單上查看標記，並依標記篩選連接清單。

* 若要依標記進行篩選，請選取 [搜尋] 方塊旁的篩選圖示。

   ![Windows Admin Center-使用搜尋方塊篩選](../media/launch/tags-7.png)

   * 您可以選取「或」、「和」或「不」來修改所選取標記的篩選器行為。

   ![Windows Admin Center 篩選連接頁面](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>使用 PowerShell 匯入或匯出您的連線 (包含標記)

[!INCLUDE [ps-connections](../includes/ps-connections.md)]

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>查看 Windows Admin Center 中使用的 PowerShell 腳本

連接到伺服器、叢集或電腦之後，您可以查看 Windows Admin Center 中提供 UI 動作的 PowerShell 腳本。 從工具內，按一下頂端應用程式行中的 [PowerShell] 圖示。 從下拉式清單中選取您感興趣的命令，以流覽至對應的 PowerShell 腳本。

![查看 PowerShell 腳本以進行總覽頁面](../media/launch/showscript.png)