---
title: 設定存放裝置報告
description: 本文說明如何設定存放裝置報告的預設參數
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f62109a8d3ea3e4e6386956789d276f9aa911e80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885229"
---
# <a name="configure-storage-reports"></a>設定存放裝置報告

> 適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

您可以設定存放裝置報告的預設參數。 這些預設參數會用於發生配額或檔案檢測事件時所產生的事件報告， 也會用於排程報告及隨選報告，您可以在定義這些報告的特定屬性時覆寫預設參數。

> [!Important]
> 當您變更特定類型報告的預設參數時，所做的變更會影響所有事件報告以及任何使用預設值的現有排程報告工作。

## <a name="to-configure-the-default-parameters-for-storage-reports"></a>若要設定存放裝置報告的預設參數

1. 在主控台樹狀目錄中，以滑鼠右鍵按一下 **\[檔案伺服器資源管理員\]**，然後按一下 **\[設定選項\]**。 [檔案伺服器資源管理員選項]  對話方塊隨即開啟。

2. 在 **\[存放裝置報告\]** 索引標籤的 **\[設定預設參數\]** 中，選取您要修改之報告的類型。

3. 按一下 **\[編輯參數\]**。

4. 視您選取的報告類型而定，可供編輯的報告參數會有所不同。 執行所有必要的修改，然後按一下 **\[確定\]**，將這些參數儲存為該種類型報告的預設參數。

5.  針對您要編輯的每個報告類型重複步驟 2 至 4。

6. 若要查看所有報告的預設參數清單，請按一下 **\[檢閱報告\]**。 然後按一下 [關閉]。

7.  按一下 [確定] 。

## <a name="see-also"></a>另請參閱

-   [設定檔案伺服器資源管理員選項](setting-file-server-resource-manager-options.md)
-   [存放裝置報告管理](storage-reports-management.md)