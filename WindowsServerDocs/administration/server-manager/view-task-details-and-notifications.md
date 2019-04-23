---
title: 檢視工作詳細資料與通知
description: 伺服器管理員
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95117407-2dd3-4f9a-841f-4331be3544c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44fd23b917d08aad663c0d3fb8da4bebef47783e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831509"
---
# <a name="view-task-details-and-notifications"></a>檢視工作詳細資料與通知

>適用於：Windows Server 2016

在 伺服器管理員在 Windows Server 2012 R2 或 Windows Server 2012，當您執行管理工作，例如新增角色和功能，請啟動服務、 重新整理資料，會顯示在 伺服器管理員 主控台中，或建立自訂伺服器群組，中會顯示通知**通知**，伺服器管理員主控台標題的區域。 通知，而**工作詳細資料**您可以從開啟的對話方塊**通知**功能表中的按一下旗標圖示，顯示使用者工作或要求的狀態、 告訴您，是否工作失敗，並協助您藉由指出有關工作失敗的詳細的錯誤訊息來疑難排解問題。

## <a name="the-notifications-area"></a>通知區域
**通知**在伺服器管理員功能表列中，旗標圖示，標示的區域會顯示在 [伺服器管理員] 中啟動的工作的結果。 通知會告知您啟動在 [伺服器管理員] 中的工作是否成功或失敗。 有可供您檢視的通知時，旗標圖示旁會顯示可用的通知數目。 如果工作失敗、只能部分完成 (例如，無法在您要執行工作的所有遠端伺服器上完成)，或是已完成但有警告，則 [通知] 旗標會變成紅色。 會顯示通知的工作如下。

-   手動重新整理顯示在 伺服器管理員 （重新整理失敗時，才，通知會顯示自動重新整理） 的資料

-   啟動或停止服務

-   安裝或解除安裝角色、 角色服務與功能

-   啟動 Best Practices Analyzer (BPA) 掃描

-   新增要管理的遠端伺服器 （無法連絡或重新整理遠端伺服器顯示的資料會顯示通知）

[通知]  功能表中的項目會顯示進度列、工作的簡短說明、工作的目標伺服器名稱 (如果選取了多部目標伺服器，則為其中一部)、相關控制項或對話方塊的連結 (如果適用)，以及 [工作]  功能表。 [工作] 功能表會顯示套用至使用中通知 (滑鼠游標暫留在其上方的通知) 的命令。 例如，如果您停止服務，可以按一下通知中 [工作]  功能表上的 [重新啟動]  來重新啟動服務。

通知是特別適用於安裝或解除安裝角色、 角色服務與功能。 比方說，如果您在遠端伺服器上啟動功能安裝，您可以關閉新增角色及功能精靈 時安裝作業仍在進行，但使用中工作仍保留在**通知**清單。 **通知**項目顯示一個進度列，進行安裝，並讓您重新開啟新增角色及功能精靈，如有需要按一下**新增角色及功能精靈**。 這份清單中的項目會通知您安裝是否失敗，或者是否需要額外的設定步驟才能完成部署功能。

通知也扮演重要部分中的工作或處理程序在 [伺服器管理員] 中的問題疑難排解。 如需有關如何使用中的訊息**通知**區域並**工作詳細資料**對話方塊來疑難排解失敗的工作或處理程序，請參閱[伺服器管理員疑難排解指南](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)。

若要刪除您不再想要看到的通知**通知**清單中，將您的滑鼠游標停留在該通知上方，，然後按一下**移除工作**(**X**)。

## <a name="viewing-and-troubleshooting-tasks-by-using-task-details"></a>檢視及疑難排解工作，藉由使用 工作詳細資料
**工作詳細資料**底部的命令**通知** 功能表隨即開啟**工作詳細資料** 對話方塊中，其中提供工作事件 （啟動的完整說明，停止、 警告、 成功或失敗）。 像其他清單控制項中 伺服器管理員 中，這類**事件**， **Services**，和**Best Practices Analyzer**圖格，您可以篩選並建立工作上執行的查詢，會顯示在**工作詳細資料** 對話方塊。 (如需有關篩選和建立查詢，在清單控制項上的詳細資訊，請參閱[篩選、 排序及查詢資料在伺服器管理員磚中](filter-sort-and-query-data-in-server-manager-tiles.md)。)在上方窗格中，您可以檢閱已在 [通知] 功能表中顯示的通知，並查看相同工作產生了多少個通知。 選取上方窗格中的 通知的下方窗格中顯示有關該通知的完整詳細資料。

下方窗格對於失敗工作的疑難排解尤其實用。 如果無法連線到伺服器管理員，或取得的伺服器集區成員的伺服器中的資料，此窗格中的項目通常會包含詳細的訊息，包括基礎 Windows 遠端管理 (WinRM)、 網路或安全性問題的完整文字，防止伺服器管理員與目標伺服器通訊。

## <a name="see-also"></a>另請參閱
[篩選、 排序和查詢在伺服器管理員磚中的資料](filter-sort-and-query-data-in-server-manager-tiles.md)
[伺服器管理員疑難排解指南](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)
