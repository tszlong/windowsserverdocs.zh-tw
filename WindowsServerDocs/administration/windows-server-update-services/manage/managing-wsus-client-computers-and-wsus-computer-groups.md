---
title: 管理 WSUS 用戶端電腦和 WSUS 電腦群組
description: Windows Server Update Service (WSUS) 主題-如何管理用戶端電腦和群組
ms.topic: article
ms.assetid: 4b1ea915-0f9f-4f0e-8913-a1dd460d07ab
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f5045df7ec213e459b5f3d2aa1b8b2dedb76b8f4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624437"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>管理 WSUS 用戶端電腦和 WSUS 電腦群組

>適用於：Windows Server 2019、Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

[電腦] 節點是 WSUS 系統管理主控台中的集中存取點，用於管理 WSUS 用戶端電腦和裝置。 在此節點下，您可以找到已設定的不同群組 (以及預設群組 [未指派的電腦]) 。

## <a name="managing-client-computers"></a>管理用戶端電腦
在 [**選項**] 下的 [**電腦**] 節點中選取其中一個電腦群組，會導致該群組中的電腦顯示在詳細資料窗格中。 如果電腦已指派給多個群組，則會出現在這兩個群組的清單中。 如果您選取清單中的電腦，您可以看到其內容，其中包括電腦的一般詳細資料，以及該電腦的更新狀態，例如特定電腦的更新安裝或偵測狀態。 您可以依狀態篩選指定電腦群組底下的電腦清單。 預設只會顯示需要更新或安裝失敗的電腦;不過，您可以依任何狀態篩選顯示。 變更狀態 **篩選之後，** 請按一下 [重新整理]。

您也可以在 [電腦] 頁面上管理電腦群組，其中包括建立群組及將電腦指派給這些群組。 如需管理電腦群組的詳細資訊，請參閱本指南下一節中的管理電腦群組，以及第 [1.5 節。在步驟1中規劃 WSUS 電腦群組](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) ：準備部署 wsus 部署指南的 wsus。

> [!NOTE]
> 您必須先設定用戶端電腦與 WSUS 伺服器連線，然後才能從該伺服器管理它們。 在您執行這項工作之前，您的 WSUS 伺服器將無法辨識您的用戶端電腦，而且不會顯示在 [電腦] 頁面的清單中。 如需設定用戶端電腦的詳細資訊，請參閱 [1.5。規劃 wsus 電腦群組](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) 的步驟1：準備部署 wsus，以及《 wsus 部署指南》中的步驟3：設定 wsus。

## <a name="controlling-when-wsus-client-computers-install-updates"></a>控制 WSUS 用戶端電腦安裝更新的時間
有兩種方法可以控制 WSUS 用戶端電腦安裝更新的時間：

-   具有期限的核准：在安裝更新時嚴格強制執行期限

-   WSUS 群組原則：當 Windows Update 代理程式掃描及安裝更新時，群組原則會進行控制

    如需詳細資訊，請參閱《 WSUS 部署指南》中的： [步驟5：設定自動更新的群組原則設定](../deploy/4-configure-group-policy-settings-for-automatic-updates.md)。

## <a name="managing-computer-groups"></a>管理電腦群組
WSUS 允許您針對用戶端電腦選擇更新，這樣就可以確保特定電腦永遠可以在最便利的時間取得正確的更新。 例如，如果一個部門 (例如會計部門) 中所有的電腦都有特定的設定，您可以為該部門設定一個群組，決定這些電腦需要哪些更新以及安裝更新的時間，然後使用 WSUS 報告來評估部門的更新。

電腦一律會指派給 [ **所有電腦** ] 群組，並保留指派給 [未指派的 **電腦** ] 群組，直到您將它們指派給另一個群組為止。 電腦可以屬於多個群組。

您可以階層方式設定電腦群組 (例如，在「會計」群組下方放置「薪資」群組和「應付帳款」群組)。 核准較高群組的更新將會自動部署到較低的群組，以及更高的群組本身。 因此，如果您核准帳戶處理群組的 Update1，則會將更新部署到會計群組中的所有電腦、薪資群組中的所有電腦，以及帳戶應付群組中的所有電腦。

因為可以將電腦指派到多個群組，所以可能為相同的電腦多次核准單一更新。 不過，只能部署更新一次，而且 WSUS 伺服器會解決任何衝突。 若要繼續進行上述範例，如果將 Wdsserver1 指派給薪資和 Accounts 帳戶的群組，而 Update1 已針對這兩個群組核准，則只會部署一次。

您可以使用下列其中一種方法，將電腦指派給電腦群組：以伺服器端為目標或以用戶端為目標。 以伺服器端為目標時，您可以手動將一或多部用戶端電腦移到一個電腦群組。 使用以用戶端為目標時，您可以使用群組原則或編輯用戶端電腦上的登錄設定，讓這些電腦自動將自己新增至先前建立的電腦群組。 此程式可同時編寫腳本並部署到多部電腦上。 您必須在 [**選項**] 頁面的 [**電腦**] 區段中，選取兩個選項的其中之一，以指定您將在 WSUS 伺服器上使用的目標方法。

> [!NOTE]
> 如果 WSUS 伺服器是在複寫模式中執行，這部伺服器上就不能建立電腦群組。 複本伺服器用戶端所需的所有電腦群組，都必須在 wsus 伺服器階層的根目錄的 WSUS 伺服器上建立。 如需有關複本模式的詳細資訊，請參閱執行 [WSUS 複本模式](running-wsus-replica-mode.md) 和伺服器端和用戶端目標的詳細資訊，請參閱第 [1.5 節。規劃 wsus 電腦群組](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) 的步驟1：準備部署 wsus 部署指南中的 wsus。


