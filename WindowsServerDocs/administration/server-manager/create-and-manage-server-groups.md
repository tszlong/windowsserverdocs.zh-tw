---
title: Create and Manage Server Groups
description: 瞭解如何在 Windows Server 伺服器管理員中建立自訂的使用者定義伺服器群組。
ms.topic: article
ms.assetid: 9d5b1be8-49fd-4ff7-9580-e4ff21fe4b17
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f1c62d0c7fba4dd23512ea5b3100dca9d099abb4
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965193"
---
# <a name="create-and-manage-server-groups"></a>建立和管理伺服器群組

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明如何在 Windows Server 伺服器管理員中建立自訂的使用者定義伺服器群組。

## <a name="server-groups"></a><a name=BKMK_groups></a>伺服器群組
您新增到伺服器集區的伺服器會顯示在伺服器管理員的 [ **所有伺服器** ] 頁面上。 您可以為您新增的伺服器建立自訂的伺服器群組。 伺服器群組可讓您以邏輯單元形式來查看和管理伺服器集區的較小子集;例如，您可以為組織會計部門中的所有伺服器建立稱為「 **會計伺服器** 」的群組，或為位於芝加哥各地的所有伺服器建立稱為「 **芝加哥** 」的群組。 在您建立伺服器群組之後，伺服器管理員中的群組首頁會顯示事件、服務、效能計數器、最佳做法分析程式結果，以及整個群組的已安裝角色和功能的相關資訊。

伺服器可以是多個群組的成員。

#### <a name="to-create-a-new-server-group"></a>建立新的伺服器群組

1.  在 [ **管理** ] 功能表上，按一下 [ **建立伺服器群組**]。

2.  在 [伺服器群組名稱] 文字方塊中，為伺服器群組輸入好記的名稱，像是「會計伺服器」。

3.  從伺服器集區將伺服器新增至 **選取** 的清單，或使用 [ **active directory**]、[ **DNS**] 或 [匯 **入** ] 索引標籤將其他伺服器新增至群組。 如需如何使用這些索引標籤的詳細資訊，請參閱本指南中的 [將伺服器新增至伺服器管理員](add-servers-to-server-manager.md) 。

4.  當您將需要的伺服器新增到該群組後，按一下 [確定]。 新群組會顯示在 [ **所有伺服器** ] 群組的伺服器管理員流覽窗格中。

#### <a name="to-edit-an-existing-server-group"></a>編輯現有的伺服器群組

1.  執行下列其中一項動作。

    -   在伺服器管理員流覽窗格中，以滑鼠右鍵按一下伺服器群組，然後按一下 [ **編輯服務器群組**]。

    -   在伺服器群組的首頁上，開啟 [**伺服器**] 磚上的 **[工作]** 功能表，然後按一下 [**編輯服務器群組**]。

2.  變更組名，或從群組中新增或移除伺服器。

    > [!NOTE]
    > 從伺服器群組移除伺服器不會從伺服器管理員移除伺服器。 您從群組中移除的伺服器仍會保留在伺服器集區的 [所有伺服器] 群組中。

3.  當您完成對群組的變更後，按一下 [確定]。

#### <a name="to-delete-an-existing-server-group"></a>刪除現有的伺服器群組

1.  執行下列其中一項動作。

    -   在 [伺服器管理員] 導覽窗格中，以滑鼠右鍵按一下伺服器群組，然後按一下 [ **刪除伺服器群組**]。

    -   在伺服器群組的首頁上，開啟 [**伺服器**] 磚上的 **[工作]** 功能表，然後按一下 [**刪除伺服器群組**]。

2.  當系統提示您是否確定要刪除伺服器群組時，按一下 [是]。

    > [!NOTE]
    > 刪除伺服器群組並不會移除伺服器管理員的伺服器。 已刪除群組中的伺服器仍會保留在伺服器集區的 [所有伺服器] 群組中。

3.  當您完成對群組的變更後，按一下 [確定]。

## <a name="see-also"></a>另請參閱
[將伺服器新增至伺服器管理員](add-servers-to-server-manager.md) 
[伺服器管理員](server-manager.md)



