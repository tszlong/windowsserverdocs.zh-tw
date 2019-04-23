---
title: 更新同步處理設定
description: Windows Server Update Service (WSUS) 主題-如何安裝和設定更新同步處理作業
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ddd5c395-451b-44a0-8e08-a05db26d2282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e381316372e68d2a43203b8fc90a243af5f40b02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869209"
---
# <a name="setting-up-update-synchronizations"></a>更新同步處理設定

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

同步處理期間，WSUS 伺服器，請從更新來源下載的更新 （更新中繼資料和檔案）。 它也會下載新的產品分類和類別，如果有的話。 當您同步處理 WSUS 伺服器第一次時，它會下載所有更新您指定當您設定同步處理選項。 第一次同步處理之後，您的 WSUS 伺服器只更新從下載的更新來源，以及現有的更新和過期的中繼資料中的修訂更新。

WSUS 伺服器下載更新的第一次可能需要很長的時間。 如果您要設定多部 WSUS 伺服器，您可以加速此程序到某個程度藉由下載一部 WSUS 伺服器上的所有更新，然後將更新複製到其他的 WSUS 伺服器的內容目錄。

您可以複製從一部 WSUS 伺服器的內容目錄的內容到另一個。 當您執行的 WSUS post 安裝程序時，將指定的內容目錄的位置。 您可以使用 wsusutil.exe 工具從一部 WSUS 伺服器的更新中繼資料匯出至檔案。 然後您可以將該檔案匯其他 WSUS 伺服器。

## <a name="setting-up-update-synchronizations"></a>更新同步處理設定
**選項**頁面是在 WSUS 管理主控台中自訂如何您的 WSUS 伺服器同步處理更新的中央存取點。 您可以指定自動同步處理的更新，您的伺服器取得更新、 連線設定和同步處理排程。 您也可以使用 「 組態精靈 」，從**選項**頁面，即可設定，或隨時重新設定您的 WSUS 伺服器。

### <a name="synchronizing-update-by-product-and-classification"></a>同步處理更新的產品和分類
WSUS 伺服器下載更新為基礎的產品或產品系列 （例如，Windows 或 Windows Server 2008 Datacenter edition） 和您指定的分類 （例如，重大更新或安全性更新）。 在第一次同步處理，WSUS 伺服器下載所有在您指定的類別中可用的更新。 在後續的同步處理，您 WSUS 伺服器下載最新的更新 （或更新您的 WSUS 伺服器上既有的變更） 的類別，指定。

您可以指定更新的產品和分類**選項**下方的頁面上**產品和分類**。 在階層中，依產品系列，列出產品。 如果您選取 [Windows] 時，自動會選取落於該產品階層每一個產品。 藉由選取 [父] 核取方塊，您選取所有項目，以及所有未來的版本。 選取子核取方塊，將不會選取父核取方塊。 預設設定為產品所有的 Windows 產品，而且分類的預設值是重大和安全性更新。

如果在複寫模式中執行的 WSUS 伺服器，您將無法執行這項工作。 如需複寫模式的詳細資訊，請參閱[執行的 WSUS 複本模式](running-wsus-replica-mode.md)，和[步驟 1:準備部署 WSUS](../plan/plan-your-wsus-deployment.md)。

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>若要指定更新產品和分類同步處理

1.  在 WSUS 管理主控台中，按一下**選項**節點。

2.  按一下 [**產品和分類**，然後按一下**產品**] 索引標籤。

3.  選取的產品或產品系列，您想要使用 WSUS，更新，然後按一下核取方塊**確定**。

4.  在 **分類**索引標籤上，選取您想要同步處理，然後按一下您的 WSUS 伺服器的更新分類的核取方塊**確定**。

> [!NOTE]
> 您可以移除產品或分類相同的方式。 您的 WSUS 伺服器會停止同步處理新的更新，您已經清除的產品。 不過，您在清除之前同步處理這些產品的更新會保留在您的 WSUS 伺服器，而且將會列出為可用。
> 
> 若要移除這些產品，拒絕更新，如中所述[更新作業](updates-operations.md)，然後使用[伺服器清理精靈](the-server-cleanup-wizard.md)將它們移除。

### <a name="synchronizing-updates-by-language"></a>同步處理語言的更新
您的 WSUS 伺服器下載更新，根據您指定的語言。 您可以同步處理中的所有可供使用、 語言的更新，或者您可以指定語言的子集。 如果您有 WSUS 伺服器階層，而且您需要以不同的語言下載更新，請確定您已指定所有必要的語言與上游伺服器上。 下游伺服器上，您可以指定您的上游伺服器所指定的語言的子集。

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>同步處理從 Microsoft Update 類別目錄更新
如需有關同步處理從 Microsoft Update 類別目錄站台更新的詳細資訊，請參閱：[WSUS 和類別目錄站台](wsus-and-the-catalog-site.md)。

### <a name="synchronizing-device-updates-by-inventory-inventory-based-synchronization"></a>同步處理清查 （清查型同步作業） 的裝置更新
某些產品類別和分類 （例如驅動程式） 包含非常大量的更新，而且我們不建議在同步處理至 WSUS 伺服器的這些整個類別。 如此一來可能會導致效能問題和持續性維護的挑戰。 WSUS 庫存系統會收集從用戶端裝置的非識別資訊並使用該清查資訊從 Microsoft Update 擷取剛好足夠更新中繼資料。 這項機制是大致上相當於具有 WSUS 自動搜尋 Microsoft Update Catalog 匯入的裝置上所偵測到更新受管理的裝置。

啟用此清查功能是支援的唯一方法，來取得特定的裝置韌體和模型為基礎的服務設定不會發行到 Microsoft Update 類別目錄。

以這種方式同步處理的更新進行檢閱並核准就像任何其他更新，並會也受限於相同的自動核准規則，取代和到期日和任何其他行為與傳統的更新。

WSUS 會執行用戶端要求特定的驅動程式和韌體更新，包括清查已匯入的更新時，伺服器端篩選。 因此，用戶端電腦或裝置會收到的中繼資料和 detectoid 驅動程式僅適用於實際連接到該裝置的裝置驅動程式更新。 此行為用戶端掃描時間降至最低，並減少用戶端與 WSUS 伺服器之間傳輸的資料。

> [!NOTE]
> 啟用清查型同步作業時，WSUS 會維護中，依每個裝置; 裝置清查只有摘要彙總 （刪除重複的檔案清單的識別碼） 會傳送至上游 WSUS 伺服器。 上游 WSUS 伺服器不會收到哪些裝置有哪些電腦，與相關聯，也不存在 WSUS 階層內的特定裝置的執行個體數目的相關資訊。 一般情況下，此摘要彙總不能用來識別或計算 WSUS 管理的網路上的裝置。

## <a name="configuring-proxy-server-settings"></a>設定 Proxy 伺服器設定
您可以設定您要使用 proxy 伺服器與上游伺服器或 Microsoft Update 的同步處理期間的 WSUS 伺服器。 只有在您的 WSUS 伺服器會執行同步處理時，會套用此設定。 根據預設您的 WSUS 伺服器會嘗試直接連線到上游伺服器或 Microsoft Update。

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>若要指定 proxy 伺服器進行同步處理

1.  在 WSUS 管理主控台中，按一下**選項**，然後按一下**更新來源和 Proxy 伺服器**。

2.  在  **Proxy 伺服器**索引標籤上，選取**同步處理時使用 proxy 伺服器**核取方塊，然後輸入伺服器名稱和連接埠號碼的 proxy 伺服器。

    > [!NOTE]
    > 將 WSUS 設定使用 proxy 伺服器設定為相同連接埠號碼使用。

    -   如果您想要連接到特定的使用者認證的 proxy 伺服器，請選取**使用使用者認證來連線到 proxy 伺服器**核取方塊，，然後輸入對應的方塊中的 使用者名稱、 網域和使用者的密碼.

    -   如果您想要為連線到 proxy 伺服器，選取的使用者啟用基本驗證**允許基本驗證 （密碼會以純文字傳送）** 核取方塊。

3.  按一下 [確定] 。

    > [!NOTE]
    > 因為 WSUS 會初始化所有網路流量，但沒有需要直接連線到 Microsoft update 的 WSUS 伺服器上設定 Windows 防火牆。

## <a name="configuring-the-update-source"></a>設定更新來源
更新來源是從中您的 WSUS 伺服器取得其更新的位置，並更新中繼資料。 您可以指定更新來源應 Microsoft Update 或另一部 WSUS 伺服器 （做為更新來源是上游伺服器，而且您的伺服器是下游伺服器的 WSUS 伺服器）。

自訂您的 WSUS 伺服器與更新來源的同步處理的選項包括：

-   您可以指定自訂連接埠進行同步處理。 如需設定通訊埠的詳細資訊，請參閱[步驟 3:將 WSUS 設定](../deploy/2-configure-wsus.md)WSUS 部署指南中。

-   您可以使用安全通訊端層 (SSL)，以安全的同步處理的 WSUS 伺服器之間的更新資訊。 如需使用 SSL 的詳細資訊，請參閱下一節 「 3.5。 安全使用 WSUS 的安全通訊端層通訊協定 」 的[步驟 3:將 WSUS 設定](../deploy/2-configure-wsus.md)WSUS 部署指南中。

## <a name="synchronizing-manually-or-automatically"></a>手動或自動同步處理
您可以手動同步處理您的 WSUS 伺服器，或指定為其自動同步處理的時機。

#### <a name="to-manually-synchronize-the-wsus-server"></a>若要手動同步處理 WSUS 伺服器

1.  在 WSUS 管理主控台中，按一下**選項**，然後按一下**同步處理排程**。

2.  按一下 **手動同步處理**，然後按一下**確定**。

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>若要設定自動同步處理排程

1.  在 WSUS 管理主控台中，按一下**選項**，然後按一下**同步處理排程**。

2.  按一下 **自動同步處理**。

3.  針對**首次同步處理**，選取您想要啟動每一天同步處理的時間。

4.  針對**每天**，選取您想要每一天同步處理的數目。 例如，如果您想同步處理 4 次一天開始，在上午 3:00，然後同步處理會發生在上午 3:00、 上午 9:00，下午 3:00 和下午 9:00 每一天。 （請注意隨機的時間位移會新增至排程的同步處理時間，以分開的伺服器連線至 Microsoft Update）。

5.  按一下 [確定] 。

#### <a name="to-synchronize-your-wsus-server-immediately"></a>若要立即同步處理您的 WSUS 伺服器

1.  在 WSUS 管理主控台中，選取最上層的伺服器節點。

2.  在 **概觀**窗格下方**同步處理狀態**，按一下**立即同步處理**。

> [!NOTE]
> 下游伺服器會起始同步處理。
