---
title: 步驟 3-在 WSUS 中核准和部署更新
description: Windows Server Update Service （WSUS）主題-在 WSUS 中核准和部署更新是部署 WSUS 的四個步驟程式中的步驟3
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 8d728ff9-170f-47e6-aefe-52be93315a75
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68ff4c893302167815e3e8368d8b03f97d9be131
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361688"
---
# <a name="step-3-approve-and-deploy-updates-in-wsus"></a>步驟 3：在 WSUS 中核准和部署更新

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

電腦群組中的電腦會在接下來的 24 小時內連絡 WSUS 伺服器來取得更新。 您可以使用 WSUS 報告功能來判斷這些更新是否已經部署到測試電腦。 在成功完成測試之後，您可以為組織中適當的電腦群組核准更新。 下列檢查清單描述使用 WSUS 管理主控台來核准和部署更新的步驟。

|工作|描述|
|----|--------|
|[3.1. 核准和部署 WSUS 更新](3-approve-and-deploy-updates-in-wsus.md#BKM_3.1.)|使用 WSUS 管理主控台來核准和部署 WSUS 更新。|
|[3.2. 設定自動核准規則](3-approve-and-deploy-updates-in-wsus.md#BKM_3.2.a.)|設定 WSUS 來自動核准安裝所選群組的更新，以及如何核准現有更新的修訂。|
|[3.3. 使用 WSUS 報告來審查已安裝的更新](3-approve-and-deploy-updates-in-wsus.md#BKM_3.3.)|使用 WSUS 報告功能檢閱已經安裝的更新、接收這些更新的電腦及其他詳細資料。|

## <a name="BKM_3.1."></a>3.1。 核准和部署 WSUS 更新
使用下列程序來核准和部署更新。

#### <a name="to-approve-and-deploy-wsus-updates"></a>核准和部署 WSUS 更新

1.  在 WSUS 管理主控台上按一下 [更新]。 右窗格會顯示 [所有更新]、[重大更新]、[安全性更新] 以及 [WSUS 更新] 的更新狀態摘要。

2.  在 [所有更新] 區段中，按一下 [電腦需要的更]。

3.  在更新清單中，選取您要核准安裝在測試電腦群組上的更新。 所選更新的相關資訊可以在 [更新] 面板的下方窗格中取得。 若要選取多個連續的更新，請按住**shift**鍵，同時按一下更新名稱。 若要選取多個不連續的更新，按住 **CTRL** 鍵同時按一下更新名稱。

4.  在選取項目上按一下滑鼠右鍵，然後按一下 [核准]。

5.  在 [核准更新] 對話方塊中，選取您的測試群組，然後按一下向下箭號。

6.  按一下 [已核准安裝]，然後按一下 [確定]。

7.  [核准進度] 視窗進度隨即出現，顯示影響更新核准的工作進度。 核准程序完成後，按一下 [關閉]。

## <a name="BKM_3.2.a."></a>3.2。 設定自動核准規則
「自動核准」可讓您指定如何自動核准安裝所選群組的更新，以及如何核准現有更新的修訂。

#### <a name="to-configure-automatic-approvals"></a>設定自動核准

1.  開啟 WSUS 管理主控台，在 [Update Services] 下展開 WSUS 伺服器，然後按一下 [選項]。 [選項] 視窗隨即開啟。

2.  在 [選項]中，按一下 [自動核准]。 [自動核准] 對話方塊隨即開啟。

3.  在 [更新規則] 中，按一下 [新增規則]。 [**新增規則**] 對話方塊隨即開啟。

4.  在 [**新增規則**] 的 [**步驟1：選取屬性**] 中，選取下列任何一個選項或選項的組合：

    -   **當特定分類中有更新時**

    -   **當特定產品中有更新時**

    -   **設定核准期限**

5.  在 **[步驟2：編輯屬性**] 中，按一下每個列出的選項，然後為每個選項選取適當的選項。

6.  在 [**步驟3：指定名稱**] 中，輸入規則的名稱，然後按一下 **[確定]** 。

7.  按一下 [確定] 關閉 [自動核准] 對話方塊。

## <a name="BKM_3.3."></a>3.3。 使用 WSUS 報告檢閱安裝的更新
更新核准 24 小時之後，您就可以使用 WSUS 報告功能來判斷更新是否部署到測試群組電腦。 若要檢查更新的狀態，您可以使用下列方式使用 WSUS 報告功能。

#### <a name="to-review-updates"></a>檢閱更新

1.  在 WSUS 管理主控台的瀏覽窗格中，按一下 [報告]。

2.  在 [報告] 頁面上，按一下 [更新狀態摘要] 報告。 隨即顯示 [更新報告] 視窗。

3.  如果您想要篩選更新清單，選取想要使用的條件，例如 [包括這些分類中的更新]，然後按一下 [執行報告]。

4.  您將會看到 [更新報告] 窗格。 您可以選取窗格左側區段中的更新，查看個別更新的狀態。 報告窗格的最後一個區段會顯示更新的狀態摘要。

5.  按一下工具列上適當的圖示，就可以儲存或列印這份報告。

6.  測試完更新之後，您可以為組織中適當的電腦群組核准更新。
