---
title: 建立和管理伺服器群組
description: 伺服器管理員
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: 9d5b1be8-49fd-4ff7-9580-e4ff21fe4b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f4ad512c55bcd1391ad55bdbdeb9a2ba3bfd7f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851541"
---
# <a name="create-and-manage-server-groups"></a>建立和管理伺服器群組

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明如何在 Windows Server 的伺服器管理員中，建立自訂的使用者定義伺服器群組。

## <a name="server-groups"></a><a name=BKMK_groups></a>伺服器群組
您新增至伺服器集區的伺服器會顯示在伺服器管理員的 [**所有伺服器**] 頁面上。 您可以為您新增的伺服器建立自訂的伺服器群組。 伺服器群組可讓您以邏輯單元的形式，來查看和管理伺服器集區的較小子集;例如，您可以為組織會計部門中的所有伺服器建立稱為「**會計伺服器**」的群組，或是針對地理位置在芝加哥的所有伺服器建立稱為「**芝加哥**」的群組。 建立伺服器群組之後，中的群組首頁伺服器管理員會顯示事件、服務、效能計數器、最佳做法分析程式結果，以及整個群組的已安裝角色和功能的相關資訊。

伺服器可以是多個群組的成員。

#### <a name="to-create-a-new-server-group"></a>建立新的伺服器群組

1.  在 [**管理**] 功能表上，按一下 [**建立伺服器群組**]。

2.  在 [伺服器群組名稱] 文字方塊中，為伺服器群組輸入好記的名稱，像是「會計伺服器」。

3.  使用 [ **active directory**]、[ **DNS**] 或 [匯**入**] 索引標籤，將伺服器從伺服器集區新增至 [已**選取**] 清單，或將其他伺服器新增至群組。 如需有關如何使用這些索引標籤的詳細資訊，請參閱本指南中的[將伺服器新增至伺服器管理員](add-servers-to-server-manager.md)。

4.  當您將需要的伺服器新增到該群組後，按一下 [確定]。 新群組會顯示在 [伺服器管理員] 流覽窗格的 [**所有伺服器**] 群組底下。

#### <a name="to-edit-an-existing-server-group"></a>編輯現有的伺服器群組

1.  執行下列其中一個步驟。

    -   在 [伺服器管理員] 流覽窗格中，以滑鼠右鍵按一下伺服器群組，然後按一下 [**編輯服務器群組**]。

    -   在伺服器群組的 [首頁] 頁面上，開啟 [**伺服器**] 磚上的 [工作 **] 功能表，然後按一下 [** **編輯服務器群組**]。

2.  變更組名，或從群組中新增或移除伺服器。

    > [!NOTE]
    > 從伺服器群組中移除伺服器並不會從伺服器管理員移除伺服器。 您從群組中移除的伺服器仍會保留在伺服器集區的 [所有伺服器] 群組中。

3.  當您完成對群組的變更後，按一下 [確定]。

#### <a name="to-delete-an-existing-server-group"></a>刪除現有的伺服器群組

1.  執行下列其中一個步驟。

    -   在 [伺服器管理員] 流覽窗格中，以滑鼠右鍵按一下伺服器群組，然後按一下 [**刪除伺服器群組**]。

    -   在伺服器群組的 [首頁] 頁面上，開啟 [**伺服器**] 磚上的 [工作 **] 功能表，** 然後按一下 [**刪除伺服器群組**]。

2.  當系統提示您是否確定要刪除伺服器群組時，按一下 [是]。

    > [!NOTE]
    > 刪除伺服器群組並不會從伺服器管理員移除伺服器。 已刪除群組中的伺服器仍會保留在伺服器集區的 [所有伺服器] 群組中。

3.  當您完成對群組的變更後，按一下 [確定]。

## <a name="see-also"></a>另請參閱
[將伺服器新增至伺服器管理員](add-servers-to-server-manager.md)
[伺服器管理員](server-manager.md)



