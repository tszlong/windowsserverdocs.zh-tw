---
title: Windows 管理中心入門
description: Windows 管理中心入門
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 02/15/2019
ms.openlocfilehash: 1643568cd1a0cdbb693d773a8357d2c36b701fd3
ms.sourcegitcommit: 7c7fc443ecd0a81bff6ed6dbeeaf4f24582ba339
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2019
ms.locfileid: "74903973"
---
# <a name="get-started-with-windows-admin-center"></a>開始使用 Windows 管理中心

>適用於：Windows Admin Center、Windows Admin Center 預覽版

> [!Tip]
> 對 Windows Admin Center 不熟悉？
> [深入了解 Windows Admin Center](../understand/windows-admin-center.md) 或[立即下載](https://aka.ms/windowsadmincenter).

## <a name="windows-admin-center-installed-on-windows-10"></a>安裝在 Windows 10 上的 windows 系統管理中心

> [!IMPORTANT]
> 您必須是本機系統管理員群組的成員，才能在 Windows 10 上使用 Windows 系統管理中心

### <a name="selecting-a-client-certificate"></a>選取用戶端憑證

當您第一次在 Windows 10 上開啟 Windows 系統管理中心時，請務必選取*Windows 管理中心用戶端*憑證（否則您會收到 HTTP 403 錯誤，指出「無法進入此頁面」）。

在 Microsoft Edge 中，當系統提示您使用此對話方塊時：
 
1. 按一下 [**更多選擇**]

    ![](../media/launch-cert-1.png)

2. 選取標示為 [ **Windows Admin Center 用戶端**] 的憑證，然後按一下 **[確定]**

    ![](../media/launch-cert-2.png)

3. 請確定已選取 [**永遠允許存取**]，然後按一下 [**允許**]

    ![](../media/launch-cert-3.png)

## <a name="connecting-to-managed-nodes-and-clusters"></a>連接到受管理的節點和叢集

當您完成 Windows 系統管理中心的安裝之後，您可以從主要的 [總覽] 頁面新增要管理的伺服器或叢集。

 **將單一伺服器或叢集新增為受管理節點**

1. 按一下 [**所有連接**] 底下的 [ **+ 新增**]。

   ![](../media/launch/addserver0.png)

2. 選擇新增伺服器、叢集、Windows 電腦或 Azure VM：
    
   ![](../media/launch/ChooseConnectionType.png)

3. 輸入要管理的伺服器或叢集名稱，然後按一下 [**提交**]。 在 [總覽] 頁面上，伺服器或叢集將會新增至您的連接清單。

   ![](../media/launch/addserver2.png)

   **--或--**

**大量匯入多部伺服器**

 1. 在 [**新增伺服器連接**] 頁面上，選擇 [匯**入伺服器**] 索引標籤。

    ![](../media/launch/import-servers.png)

 2. 按一下 **[流覽]** ，然後選取含有逗號或換行的文字檔，其中包含您想要新增之伺服器的 fqdn 清單。

> [!Note]
> 藉由[使用 PowerShell 匯出](#use-powershell-to-import-or-export-your-connections-with-tags)連線所建立的 .csv 檔案包含伺服器名稱以外的其他資訊，而且與此匯入方法不相容。

  **--或--**

**藉由搜尋來新增伺服器 Active Directory**

 1. 在 [**新增伺服器連接**] 頁面上，選擇 [**搜尋 Active Directory] 索引**標籤。

    ![](../media/launch/search-ad.png)

 2. 輸入您的搜尋條件，然後按一下 [**搜尋**]。 支援萬用字元（*）。

 3. 搜尋完成之後-選取一或多個結果，並選擇性地新增標籤，然後按一下 [**新增**]。

## <a name="authenticate-with-the-managed-node"></a>使用受管理的節點進行驗證 ##

Windows 系統管理中心支援數個使用受管理節點進行驗證的機制。 [單一登入] 是預設值。

**單一登入**

您可以使用目前的 Windows 認證，向受管理的節點進行驗證。 這是預設值，而且 Windows 管理中心會在您新增伺服器時嘗試登入。 

**在 Windows Server 上部署為服務時的單一登入**

如果您已在 Windows Server 上安裝 Windows 系統管理中心，則需要額外的設定才能進行單一登入。  [設定您的環境以進行委派](../configure/user-access-control.md)

**--或--**

**使用 [*管理*身分] 來指定認證**

在 [**所有**連線] 底下，從清單中選取伺服器，然後選擇 [**管理**身分]，指定您將用來向受管理節點進行驗證的認證：

![](../media/launch-use-6.png)

如果 Windows 系統管理中心是在 Windows Server 上以服務模式執行，但是您未設定 Kerberos 委派，則必須重新輸入您的 Windows 認證：

![](../media/launch-use-7.png)

您可以將認證套用至所有連線，這會針對該特定瀏覽器會話進行快取。 如果您重載瀏覽器，則必須重新輸入您的 [**管理**身分] 認證。

**本機系統管理員密碼解決方案（LAPS）**

如果您的環境使用[LAPS](https://technet.microsoft.com/mt227395.aspx)，而且您的 WINDOWS 10 電腦上已安裝 Windows 系統管理中心，則可以使用 LAPS 認證來向受管理的節點進行驗證。 **如果您使用此案例，請**[提供意見](https://aka.ms/WACFeedback)反應。

## <a name="using-tags-to-organize-your-connections"></a>使用標記來組織您的連接

您可以使用標記來識別和篩選連接清單中的相關伺服器。  這可讓您在 [連線] 清單中看到伺服器的子集。  如果您有許多連線，這會特別有用。

### <a name="edit-tags"></a>編輯標記

* 在 [所有連接] 清單中選取伺服器或多部伺服器
* 按一下 [**所有連接**] 底下的 [**編輯標記**]

![](../media/launch/tags-5.png)

[**編輯連接標記**] 窗格可讓您修改、新增或移除所選連接中的標記：

* 若要將新標籤新增至選取的連接，請選取 [**新增**標籤]，然後輸入您想要使用的標記名稱。

* 若要使用現有的標記名稱標記選取的連接，請核取您要套用之標記名稱旁的方塊。

* 若要從所有選取的連線中移除標記，請取消選取您要移除之標籤旁的方塊。

* 如果將標籤套用至選取之連接的子集，則會以中繼狀態顯示此核取方塊。 您可以按一下方塊進行檢查，並將標籤套用到所有選取的連線，或再按一次以取消核取，然後從所有選取的連線移除標記。

![](../media/launch/tags-6.png)

### <a name="filter-connections-by-tag"></a>依標記篩選連接

一旦將標記加入至一或多個伺服器連接之後，您就可以在連接清單上查看標記，並依標記篩選連接清單。

* 若要依標記篩選，請選取 [搜尋] 方塊旁邊的篩選圖示。
![](../media/launch/tags-7.png)
* 您可以選取 "or"、"and" 或 "not"，以修改所選標記的篩選行為。
![](../media/launch/tags-8.png)

## <a name="use-powershell-to-import-or-export-your-connections-with-tags"></a>使用 PowerShell 匯入或匯出您的連線（含標籤）

[!INCLUDE [ps-connections](../includes/ps-connections.md)]

## <a name="view-powershell-scripts-used-in-windows-admin-center"></a>查看 Windows 系統管理中心內使用的 PowerShell 腳本

連線到伺服器、叢集或電腦之後，您可以查看 PowerShell 腳本，以增強 Windows 管理中心提供的 UI 動作。 從工具內，按一下頂端應用程式行中的 [PowerShell] 圖示。 從下拉式清單中選取想要的命令，以流覽至對應的 PowerShell 腳本。

![](../media/launch/showscript.png)
