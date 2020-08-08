---
title: 設定存放裝置報告
description: 本文說明如何設定存放裝置報告的預設參數
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8b9d6a53b5f34c0c053de860895f5c4e48b07c83
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961492"
---
# <a name="configure-storage-reports"></a>設定存放裝置報告

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

您可以設定存放裝置報告的預設參數。 這些預設參數會用於發生配額或檔案檢測事件時所產生的事件報告， 也會用於排程報告及隨選報告，您可以在定義這些報告的特定屬性時覆寫預設參數。

> [!Important]
> 當您變更特定類型報告的預設參數時，所做的變更會影響所有事件報告以及任何使用預設值的現有排程報告工作。

## <a name="to-configure-the-default-parameters-for-storage-reports"></a>若要設定存放裝置報告的預設參數

1. 在主控台樹狀目錄中，以滑鼠右鍵按一下 **\[檔案伺服器資源管理員\]**，然後按一下 **\[設定選項\]**。 [檔案伺服器資源管理員選項]**** 對話方塊隨即開啟。

2. 在 **\[存放裝置報告\]** 索引標籤的 **\[設定預設參數\]** 中，選取您要修改之報告的類型。

3. 按一下 [**編輯參數**]。

4. 視您選取的報告類型而定，可供編輯的報告參數會有所不同。 執行所有必要的修改，然後按一下 **\[確定\]**，將這些參數儲存為該種類型報告的預設參數。

5.  針對您要編輯的每個報告類型重複步驟 2 至 4。

6. 若要查看所有報告的預設參數清單，請按一下 **\[檢閱報告\]**。 然後按一下 [關閉]****。

7.  按一下 [確定]  。

## <a name="additional-references"></a>其他參考資料

-   [設定檔案伺服器資源管理員選項](setting-file-server-resource-manager-options.md)
-   [存放裝置報告管理](storage-reports-management.md)