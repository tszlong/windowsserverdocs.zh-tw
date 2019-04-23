---
title: 管理 WSUS 用戶端電腦和 WSUS 電腦群組
description: Windows Server Update Service (WSUS) 主題-如何管理用戶端電腦和群組
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b1ea915-0f9f-4f0e-8913-a1dd460d07ab
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: e63aa5d53f01fbff3029c6896556d92c9c99aa50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857679"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>管理 WSUS 用戶端電腦和 WSUS 電腦群組

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

[電腦] 節點是在 WSUS 系統管理主控台來管理 WSUS 用戶端電腦和裝置的集中存取點。 此節點下，您可以找到您已設定不同的群組 （加上預設的群組，尚未指派的電腦）。

## <a name="managing-client-computers"></a>管理用戶端電腦
選取其中一個電腦群組中**電腦**下方的節點**選項**電腦就會在 [詳細資料] 窗格中顯示該群組中。 如果電腦被指派給多個群組，它會出現在這兩個群組的清單中。 如果您在清單中選取電腦，您可以看到其屬性，其中包含有關電腦的更新，例如安裝的狀態或特定電腦的更新的偵測狀態的一般詳細資料。 您可以依狀態篩選特定的電腦群組下的電腦的清單。 預設值會顯示唯一的電腦所需的更新，或其已安裝失敗;不過，您可以依任何狀態來篩選顯示。 按一下 **重新整理**之後變更的狀態篩選。

您也可以管理 [電腦] 頁面，其中包含建立群組，並為其指派的電腦上的電腦群組。 如需管理電腦群組的詳細資訊，請參閱 < 管理電腦群組中的下一步 本指南中，區段和區段[1.5。規劃 WSUS 電腦群組](../plan/plan-your-wsus-deployment.md#BKMK_1.5)在步驟 1:準備部署 WSUS 的 WSUS 部署指南 》。

> [!NOTE]
> 您必須先設定連絡 WSUS 伺服器，才能從該伺服器管理的用戶端電腦。 直到您執行這項工作中，您的 WSUS 伺服器將無法辨識您的用戶端電腦，並不會顯示在 [電腦] 頁面的清單中。 如需設定用戶端電腦的詳細資訊，請參閱[1.5。規劃 WSUS 電腦群組](../plan/plan-your-wsus-deployment.md#BKMK_1.5)的步驟 1:準備部署 WSUS，及步驟 3:設定 WSUS，WSUS 部署指南中。

## <a name="controlling-when-wsus-client-computers-install-updates"></a>控制 WSUS 用戶端電腦安裝更新的時機
當 WSUS 用戶端電腦安裝更新時，有兩種方法可以控制：

-   使用期限的核准：安裝更新時，嚴格地強制執行期限

-   WSUS 群組原則：群組原則控制當 Windows Update 代理程式掃描，並安裝更新

    如需詳細資訊，請參閱：[步驟 5：設定自動更新的群組原則設定](../deploy/4-configure-group-policy-settings-for-automatic-updates.md)，WSUS 部署指南中。

## <a name="managing-computer-groups"></a>管理電腦群組
WSUS 允許您針對用戶端電腦選擇更新，這樣就可以確保特定電腦永遠可以在最便利的時間取得正確的更新。 例如，如果一個部門 (例如會計部門) 中所有的電腦都有特定的設定，您可以為該部門設定一個群組，決定這些電腦需要哪些更新以及安裝更新的時間，然後使用 WSUS 報告來評估部門的更新。

電腦一律指派到**的所有電腦**群組，並保持指派給**未指派的電腦**群組之前將它們指派給另一個群組。 電腦可以屬於多個群組。

您可以階層方式設定電腦群組 (例如，在「會計」群組下方放置「薪資」群組和「應付帳款」群組)。 針對較高群組核准的更新將自動部署至較低的群組，以及用於較高群組本身。 因此，如果您為 「 會計 」 群組核准 Update1，更新會部署到 「 會計 」 群組中的所有電腦、 「 薪資 」 群組中的所有電腦和帳款 」 群組中的所有電腦。

因為可以將電腦指派到多個群組，所以可能為相同的電腦多次核准單一更新。 不過，只能部署更新一次，而且 WSUS 伺服器會解決任何衝突。 若要繼續上面的範例，如果 computerA 指派到薪資和 Accounts Payable 的群組，而且這兩個群組核准 Update1，它將會部署一次。

您可以使用下列其中一種方法，將電腦指派給電腦群組：以伺服器端為目標或以用戶端為目標。 使用伺服器端為目標，您以手動方式移動一或多個用戶端電腦至一部電腦群組一次。 使用用戶端為目標，您可以使用群組原則或編輯用戶端電腦上的登錄設定，讓這些電腦自動新增到先前建立的電腦群組。 此程序可以編寫指令碼，並一次部署到多部電腦。 您必須指定目標的方法，您將使用在 WSUS 伺服器，選取兩個選項的其中一項上**電腦**一節**選項**頁面。

> [!NOTE]
> 如果 WSUS 伺服器是在複寫模式中執行，這部伺服器上就不能建立電腦群組。 必須是 WSUS 伺服器階層的根 WSUS 伺服器上建立複本伺服器的用戶端所需的所有電腦群組。 如需複寫模式的詳細資訊，請參閱[執行的 WSUS 複本模式](running-wsus-replica-mode.md)如需伺服器端和用戶端為目標的詳細資訊，請參閱下一節和[1.5。規劃 WSUS 電腦群組](../plan/plan-your-wsus-deployment.md#BKMK_1.5)的步驟 1:在 WSUS 部署指南中，以準備部署 WSUS。


