---
title: 更新同步處理設定
description: Windows Server Update Service (WSUS) 主題-如何設定和設定更新同步處理
ms.topic: article
ms.assetid: ddd5c395-451b-44a0-8e08-a05db26d2282
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 20abe0b5a8d4530bba2cd14afeec5b482574af82
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624392"
---
# <a name="setting-up-update-synchronizations"></a>更新同步處理設定

>適用於：Windows Server 2019、Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在同步處理期間，WSUS 伺服器會下載更新 (更新來源中) 的中繼資料和檔案。 它也會下載新的產品分類和類別（如果有的話）。 當 WSUS 伺服器第一次進行同步處理時，將會下載您在設定同步處理選項時所指定的所有更新。 在第一次同步處理之後，您的 WSUS 伺服器只會下載來自更新來源的更新，以及現有更新的中繼資料修訂和更新到期時間。

當 WSUS 伺服器第一次下載更新時，可能需要很長的時間。 如果您要設定多個 WSUS 伺服器，您可以藉由下載一部 WSUS 伺服器上的所有更新，然後將更新複製到其他 WSUS 伺服器的內容目錄，來加快進程的範圍。

您可以將內容從一個 WSUS 伺服器的內容目錄複製到另一個。 當您執行 WSUS 後續安裝程式時，會指定內容目錄的位置。 您可以使用 wsusutil.exe 工具，將更新中繼資料從一部 WSUS 伺服器匯出到檔案。 然後，您可以將該檔案匯入到其他 WSUS 伺服器。

## <a name="setting-up-update-synchronizations"></a>更新同步處理設定
[ **選項** ] 頁面是 Wsus 管理主控台中的中央存取點，可用於自訂 wsus 伺服器同步處理更新的方式。 您可以指定要自動同步處理的更新、伺服器取得更新的位置、連接設定，以及同步處理排程。 您也可以使用 [ **選項** ] 頁面上的 [設定] 來設定或重新設定 WSUS 伺服器。

### <a name="synchronizing-update-by-product-and-classification"></a>依產品和分類同步處理更新
WSUS 伺服器會根據產品或產品系列來下載更新 (例如，Windows 或 Windows Server 2008、Datacenter edition) 和分類 (例如，您指定的重大更新或安全性更新) 。 在第一次同步處理時，WSUS 伺服器會下載您已指定的類別中所有可用的更新。 在後續的同步處理中，您的 WSUS 伺服器只會下載最新的更新 (或您的 WSUS 伺服器上已有的更新變更，) 適用于您已指定的類別。

您可以在 [**產品和分類**] 下的 [**選項**] 頁面上，指定更新產品和分類。 產品會列在階層中，並依產品系列進行分組。 如果您選取 [Windows]，則會自動選取屬於該產品階層的每個產品。 選取 [父系] 核取方塊，即可選取其下的所有專案，以及所有未來版本。 選取子核取方塊並不會選取父核取方塊。 [產品] 的預設設定為 [所有 Windows 產品]，而 [分類] 的預設設定為 [重大] 和 [安全性更新]。

如果 WSUS 伺服器是在複本模式中執行，您將無法執行這項工作。 如需有關複本模式的詳細資訊，請參閱執行 [Wsus 複本模式](running-wsus-replica-mode.md)和 [步驟1：準備部署 wsus](../plan/plan-your-wsus-deployment.md)。

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>若要指定同步處理的更新產品和分類

1.  在 WSUS 管理主控台中，按一下 [ **選項** ] 節點。

2.  按一下 [ **產品和分類**]，然後按一下 [ **產品** ] 索引標籤。

3.  選取您要使用 WSUS 更新之產品或產品系列的核取方塊，然後按一下 **[確定]**。

4.  在 [ **分類** ] 索引標籤上，選取您要 WSUS 伺服器同步處理的更新分類核取方塊，然後按一下 **[確定]**。

> [!NOTE]
> 您可以用同樣的方式移除產品或分類。 您的 WSUS 伺服器會停止同步處理您已清除之產品的新更新。 不過，在您清除這些產品之前同步處理的更新，將會保留在您的 WSUS 伺服器上，而且將會列為可用。
>
> 若要移除這些產品，請拒絕更新（如 [更新作業](updates-operations.md)中所述），然後使用 [[伺服器清除] 嚮導](the-server-cleanup-wizard.md) 移除它們。

### <a name="synchronizing-updates-by-language"></a>依語言同步處理更新
您的 WSUS 伺服器會根據您指定的語言下載更新。 您可以同步處理所有可用語言的更新，也可以指定語言的子集。 如果您有 WSUS 伺服器的階層，而且需要下載不同語言的更新，請確定您已在上游伺服器上指定所有必要的語言。 在下游伺服器上，您可以指定在上游伺服器上指定的語言子集。

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>從 Microsoft Update 目錄同步處理更新
如需有關從 Microsoft Update 目錄網站同步處理更新的詳細資訊，請參閱： [WSUS 和目錄網站](wsus-and-the-catalog-site.md)。

## <a name="configuring-proxy-server-settings"></a>設定 Proxy 伺服器設定
您可以在與上游伺服器或 Microsoft Update 的同步處理期間，將 WSUS 伺服器設定為使用 proxy 伺服器。 只有當您的 WSUS 伺服器執行同步處理時，才會套用此設定。 根據預設，您的 WSUS 伺服器會嘗試直接連線到上游伺服器或 Microsoft Update。

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>若要指定同步處理的 proxy 伺服器

1.  在 WSUS 管理主控台中，按一下 [ **選項**]，然後按一下 [ **更新來源和 Proxy 伺服器**]。

2.  在 [ **Proxy 伺服器** ] 索引標籤上，選取 [ **同步處理時使用 Proxy 伺服器** ] 核取方塊，然後輸入 Proxy 伺服器的伺服器名稱和埠號碼。

    > [!NOTE]
    > 使用 proxy 伺服器設定使用的相同埠號碼來設定 WSUS。

    -   如果您想要使用特定使用者認證連線到 proxy 伺服器，請選取 [ **使用使用者認證連線到 proxy 伺服器]** 核取方塊，然後在對應的方塊中輸入使用者的使用者名稱、網域和密碼。

    -   如果您想要為連線到 proxy 伺服器的使用者啟用基本驗證，請選取 [ **允許基本驗證 (密碼以純文字傳送) ** ] 核取方塊。

3.  按一下 [確定]  。

    > [!NOTE]
    > 因為 WSUS 會起始其所有網路流量，所以不需要在直接連線到 Microsoft update 的 WSUS 伺服器上設定 Windows 防火牆。

## <a name="configuring-the-update-source"></a>設定更新來源
更新來源是您的 WSUS 伺服器取得更新和更新中繼資料的位置。 您可以指定更新來源應該是 Microsoft Update 或另一部 WSUS 伺服器 (作為更新來源的 WSUS 伺服器是上游伺服器，而且您的伺服器是下游伺服器) 。

自訂 WSUS 伺服器與更新來源進行同步處理的選項包括下列各項：

-   您可以指定自訂埠來進行同步處理。 如需設定埠的詳細資訊，請參閱《 WSUS 部署指南》中的 [步驟3：設定 wsus](../deploy/2-configure-wsus.md) 。

-   您可以使用安全通訊端層 (SSL) 安全地同步處理 WSUS 伺服器之間的更新資訊。 如需使用 SSL 的詳細資訊，請參閱第3.5 節。 使用「WSUS 部署指南」中的「 [步驟3：設定 wsus](../deploy/2-configure-wsus.md) 」安全通訊端層通訊協定來保護 wsus。

## <a name="synchronizing-manually-or-automatically"></a>手動或自動同步處理
您可以手動同步處理您的 WSUS 伺服器，或指定時間以自動同步處理。

#### <a name="to-manually-synchronize-the-wsus-server"></a>手動同步處理 WSUS 伺服器

1.  在 WSUS 管理主控台中，按一下 [ **選項**]，然後按一下 **[同步處理排程**]。

2.  按一下 [ **手動同步處理**]，然後按一下 **[確定]**。

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>設定自動同步處理排程

1.  在 WSUS 管理主控台中，按一下 [ **選項**]，然後按一下 **[同步處理排程**]。

2.  按一下 [ **自動同步處理**]。

3.  若為 **第一次同步**處理，請選取每天要同步處理的啟動時間。

4.  針對 **[每天同步處理]**，選取您每天要進行的同步處理數目。 例如，如果您想要在一天上午3:00 開始進行四次同步處理，則會在上午3:00、9:00 A.M.、下午3:00 和下午9:00 進行同步處理。 每天。  (請注意，隨機時間位移將會加入至排程的同步處理時間，以便將伺服器連接的空間設定為 Microsoft Update。 ) 

5.  按一下 [確定]  。

#### <a name="to-synchronize-your-wsus-server-immediately"></a>立即同步處理您的 WSUS 伺服器

1.  在 WSUS 管理主控台中，選取最上層伺服器節點。

2.  在 [ **總覽** ] 窗格的 [ **同步處理狀態**] 下，按一下 [ **立即同步處理**]。

> [!NOTE]
> 同步處理是由下游伺服器起始。
