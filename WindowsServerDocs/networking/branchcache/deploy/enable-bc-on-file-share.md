---
title: 啟用檔案共用的 BranchCache (選用)
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7528c00633c17d33dd1d09b23db8a56a3eba5a65
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971815"
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>啟用檔案共用的 BranchCache (選用)

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用這個程式，在檔案共用上啟用 BranchCache。

> [!IMPORTANT]
> 如果您使用 [**允許所有共用資料夾的雜湊發行**] 值設定雜湊發行集設定，則不需要執行此程式。

若要執行此程式，至少需要**Administrators**的成員資格或同等許可權。

### <a name="to-enable-branchcache-on-a-file-share"></a>在檔案共用上啟用 BranchCache

1.  開啟 Windows PowerShell，然後輸入 **mmc**，再按 ENTER 鍵。 此時會開啟 Microsoft Management Console (MMC)。

2.  在 MMC 的 **[檔案]** 功能表上，按一下 **[新增/移除嵌入式管理單元]**。 [**新增或移除嵌入式管理單元**] 對話方塊隨即開啟。

3.  在 [**新增或移除嵌入式管理單元**] 的 [可用的嵌入式**管理**單元] 中，按兩下 [**共用資料夾**]。 [共用資料夾] Wizard 隨即開啟，並已選取 [本機電腦] 物件。 設定您偏好的視圖，按一下 **[完成]**，然後按一下 **[確定]**。

4.  按兩下 [**共用資料夾] (本機) **]，然後按一下 [**共用**]。

5.  在詳細資料窗格中，以滑鼠右鍵按一下共用，然後按一下 [**屬性**]。 共用的 [**屬性**] 對話方塊隨即開啟。

6.  在 [**屬性**] 對話方塊的 [**一般**] 索引標籤上，按一下 [**離線設定**]。 [**離線設定**] 對話方塊隨即開啟。

7.  確定已選取 **[只有使用者指定的檔案和程式可供離線使用**]，然後按一下 [**啟用 BranchCache**]。

8.  按兩次 **[確定]**。


