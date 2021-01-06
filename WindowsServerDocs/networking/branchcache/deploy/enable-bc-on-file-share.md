---
title: 啟用檔案共用的 BranchCache (選用)
description: 瞭解如何在檔案共用上啟用 BranchCache。
manager: brianlic
ms.topic: how-to
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: d66192168cffcdab54d61c659fd53084d5b12a5a
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946714"
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>啟用檔案共用的 BranchCache (選用)

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用這個程式在檔案共用上啟用 BranchCache。

> [!IMPORTANT]
> 如果您使用 [ **允許所有共用資料夾的雜湊發行**] 值來設定雜湊發行設定，就不需要執行此程式。

若要執行此程式，至少需要 **Administrators** 的成員資格或同等許可權。

### <a name="to-enable-branchcache-on-a-file-share"></a>在檔案共用上啟用 BranchCache

1.  開啟 Windows PowerShell，然後輸入 **mmc**，再按 ENTER 鍵。 此時會開啟 Microsoft Management Console (MMC)。

2.  在 MMC 的 **[檔案]** 功能表上，按一下 **[新增/移除嵌入式管理單元]**。 [ **新增或移除嵌入式管理單元** ] 對話方塊隨即開啟。

3.  在 [ **新增或移除嵌入式管理單元**] 的 [可用的嵌入式 **管理** 單元] 中，按兩下 [ **共用資料夾**]。 [共用資料夾] Wizard 隨即開啟，並選取 [本機電腦] 物件。 設定您偏好的視圖、按一下 **[完成**]，然後按一下 **[確定]**。

4.  按兩下 [ **共用資料夾 (本機)**]，然後按一下 [ **共用**]。

5.  在詳細資料窗格中，以滑鼠 **按右鍵共用**，然後按一下 [內容]。 共用的 [ **屬性** ] 對話方塊隨即開啟。

6.  在 [ **屬性** ] 對話方塊的 [ **一般** ] 索引標籤上，按一下 [ **離線設定**]。 [ **離線設定** ] 對話方塊隨即開啟。

7.  確定 **只選取 [使用者指定的檔案和程式可供離線使用** ]，然後按一下 [ **啟用 BranchCache**]。

8.  按兩次 **[確定]**。


