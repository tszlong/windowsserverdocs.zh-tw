---
title: 管理 WSUS 用戶端電腦和 WSUS 電腦群組
description: Windows Server Update Service （WSUS）主題-如何管理用戶端電腦和群組
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 4b1ea915-0f9f-4f0e-8913-a1dd460d07ab
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 3ac5a0c09d28de53430ded651b6da92ba92a1f68
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828622"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>管理 WSUS 用戶端電腦和 WSUS 電腦群組

>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

[電腦] 節點是 WSUS 管理主控台中用來管理 WSUS 用戶端電腦和裝置的集中存取點。 在此節點下，您可以找到已設定的不同群組（加上預設群組 [未指派的電腦]）。

## <a name="managing-client-computers"></a>管理用戶端電腦
在 [**選項**] 下的 [**電腦**] 節點中選取其中一個電腦群組，會導致該群組中的電腦顯示在詳細資料窗格中。 如果將電腦指派給多個群組，它會出現在這兩個群組的清單中。 如果您在清單中選取電腦，則可以看到其內容，其中包括電腦的一般詳細資料和其更新狀態，例如特定電腦的更新安裝或偵測狀態。 您可以依狀態篩選指定電腦群組下的電腦清單。 預設只會顯示需要更新或安裝失敗的電腦;不過，您可以依任何狀態篩選顯示。 變更狀態**篩選後，** 按一下 [重新整理]。

您也可以在 [電腦] 頁面上管理電腦群組，其中包括建立群組並將電腦指派給它們。 如需管理電腦群組的詳細資訊，請參閱本指南下一節的管理電腦群組和第[1.5 節。規劃 wsus 電腦群組](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups)的步驟1：準備部署 wsus 部署指南的 wsus。

> [!NOTE]
> 您必須先設定用戶端電腦與 WSUS 伺服器的連線，才能從該伺服器進行管理。 在執行此工作之前，您的 WSUS 伺服器將無法辨識您的用戶端電腦，而且它們不會顯示在 [電腦] 頁面上的清單中。 如需設定用戶端電腦的詳細資訊，請參閱[1.5。規劃 wsus 電腦群組](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups)的步驟1：準備 Wsus 部署和步驟3：設定 wsus，位於《 WSUS 部署指南》中。

## <a name="controlling-when-wsus-client-computers-install-updates"></a>控制 WSUS 用戶端電腦安裝更新的時機
有兩種方法可以控制 WSUS 用戶端電腦安裝更新的時機：

-   具有期限的核准：在安裝更新時嚴格地強制執行期限

-   WSUS 群組原則：群組原則控制 Windows Update 代理程式掃描和安裝更新的時間

    如需詳細資訊，請參閱《 WSUS 部署指南》中的：[步驟5：設定自動更新的群組原則設定](../deploy/4-configure-group-policy-settings-for-automatic-updates.md)。

## <a name="managing-computer-groups"></a>管理電腦群組
WSUS 允許您針對用戶端電腦選擇更新，這樣就可以確保特定電腦永遠可以在最便利的時間取得正確的更新。 例如，如果一個部門 (例如會計部門) 中所有的電腦都有特定的設定，您可以為該部門設定一個群組，決定這些電腦需要哪些更新以及安裝更新的時間，然後使用 WSUS 報告來評估部門的更新。

電腦一律會指派給 [**所有電腦**] 群組，並保持指派給 [未指派的**電腦**] 群組，直到您將它們指派給另一個群組為止。 電腦可以屬於多個群組。

您可以階層方式設定電腦群組 (例如，在「會計」群組下方放置「薪資」群組和「應付帳款」群組)。 針對較高群組核准的更新會自動部署至較低的群組，以及更高的群組本身。 因此，如果您核准會計群組的 Update1，更新將會部署到 Accounting 群組中的所有電腦、[薪資] 群組中的所有電腦，以及 [帳戶應付] 群組中的所有電腦。

因為可以將電腦指派到多個群組，所以可能為相同的電腦多次核准單一更新。 不過，只能部署更新一次，而且 WSUS 伺服器會解決任何衝突。 若要繼續進行上述範例，如果將 computerA 指派給薪資和帳戶應付群組，而同時核准這兩個群組的 Update1，則只會部署一次。

您可以使用下列其中一種方法，將電腦指派給電腦群組：以伺服器端為目標或以用戶端為目標。 以伺服器端為目標時，您一次可以手動將一或多部用戶端電腦移到一個電腦群組。 以用戶端為目標時，您可以使用群組原則或編輯用戶端電腦上的登錄設定，讓這些電腦自動將自己新增到先前建立的電腦群組。 此程式可以編寫腳本，並同時部署到許多電腦上。 您必須在 [**選項**] 頁面的 [**電腦**] 區段上，選取兩個選項的其中一個，以指定您將在 WSUS 伺服器上使用的目標方法。

> [!NOTE]
> 如果 WSUS 伺服器是在複寫模式中執行，這部伺服器上就不能建立電腦群組。 複本伺服器的用戶端所需的所有電腦群組，都必須在 wsus 伺服器階層的根目錄的 WSUS 伺服器上建立。 如需有關「複本模式」的詳細資訊，請參閱執行[WSUS 複本模式](running-wsus-replica-mode.md)，如需伺服器端和用戶端目標的詳細資訊，請參閱第[1.5 節。](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups)在《 wsus 部署指南》中規劃 wsus 電腦群組的步驟1：準備部署 wsus。


