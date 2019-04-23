---
title: 建立和管理伺服器群組
description: 伺服器管理員
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d5b1be8-49fd-4ff7-9580-e4ff21fe4b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32e20040e2cb075e447c0d03d48676c7011a5a92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889479"
---
# <a name="create-and-manage-server-groups"></a>建立和管理伺服器群組

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題描述如何在 伺服器管理員在 Windows Server 中建立自訂的使用者定義伺服器群組。

## <a name="BKMK_groups"></a>伺服器群組
您將新增至伺服器集區的伺服器會顯示在**所有伺服器**頁面在 [伺服器管理員] 中。 您可以為您新增的伺服器建立自訂的伺服器群組。 伺服器群組可讓您檢視和管理您的伺服器集區，做為邏輯單元; 較小的子集比方說，您可以在其中建立群組，稱為**會計伺服器**組織中的所有伺服器的會計部門或群組稱為**芝加哥**地理位置的所有伺服器在芝加哥。 建立伺服器群組之後，群組的頁面在 [伺服器管理員] 中顯示事件、 服務、 效能計數器、 最佳做法分析程式結果的相關資訊，並整體安裝角色和群組的功能。

伺服器可以是多個群組的成員。

#### <a name="to-create-a-new-server-group"></a>建立新的伺服器群組

1.  在 **管理**功能表上，按一下**建立伺服器群組**。

2.  在 [伺服器群組名稱]  文字方塊中，為伺服器群組輸入好記的名稱，像是「會計伺服器」 。

3.  將伺服器新增到**選定**列出從伺服器集區，或將其他伺服器新增至群組中，使用**active directory**， **DNS**，或**匯入**索引標籤。 如需如何使用這些索引標籤的詳細資訊，請參閱[將伺服器新增到伺服器管理員](add-servers-to-server-manager.md)本指南中。

4.  當您將需要的伺服器新增到該群組後，按一下 [確定] 。 新的群組會顯示在 伺服器管理員瀏覽窗格下**所有伺服器**群組。

#### <a name="to-edit-an-existing-server-group"></a>編輯現有的伺服器群組

1.  執行下列其中一項。

    -   在 [伺服器管理員瀏覽] 窗格中，以滑鼠右鍵按一下某個伺服器群組，然後**編輯伺服器群組**。

    -   在的伺服器首頁上，開啟**任務**功能表上的**伺服器**圖格，然後按一下 **編輯伺服器群組**。

2.  變更群組名稱，或新增或移除群組中的伺服器。

    > [!NOTE]
    > 從伺服器群組移除伺服器不會移除伺服器從伺服器管理員。 您從群組中移除的伺服器仍會保留在伺服器集區的 [所有伺服器]  群組中。

3.  當您完成對群組的變更後，按一下 [確定] 。

#### <a name="to-delete-an-existing-server-group"></a>刪除現有的伺服器群組

1.  執行下列其中一項。

    -   在 [伺服器管理員瀏覽] 窗格中，以滑鼠右鍵按一下某個伺服器群組，然後**刪除伺服器群組**。

    -   在的伺服器首頁上，開啟**任務**功能表上的**伺服器**圖格，然後按一下 **刪除伺服器群組**。

2.  當系統提示您是否確定要刪除伺服器群組時，按一下 [是] 。

    > [!NOTE]
    > 刪除伺服器群組不會移除伺服器從伺服器管理員。 已刪除群組中的伺服器仍會保留在伺服器集區的 [所有伺服器]  群組中。

3.  當您完成對群組的變更後，按一下 [確定] 。

## <a name="see-also"></a>另請參閱
[將伺服器新增到伺服器管理員](add-servers-to-server-manager.md)
[伺服器管理員](server-manager.md)



