---
title: WSUS 和類別目錄網站
description: Windows Server Update Service （WSUS）主題-如何藉由存取 Microsoft Update 目錄網站，將修補程式匯入 WSUS
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f19a8659-5a96-4fdd-a052-29e4547fe51a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c30fad5f38a1b3088c6b4d12ac92d8b8a15aee83
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361446"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS 和類別目錄網站

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

目錄網站是您可以從中匯入修補程式和硬體驅動程式的 Microsoft 位置。

## <a name="the-microsoft-update-catalog-site"></a>Microsoft Update 目錄網站
為了將修補程式匯入 WSUS，您必須從 WSUS 電腦存取 Microsoft Update 目錄網站。 任何已安裝 WSUS 系統管理主控台的電腦（不論是否為 WSUS 伺服器）都可以用來從類別目錄網站匯入修補程式。 您必須以系統管理員的身分登入電腦，才能匯入修補程式。

#### <a name="to-access-the-microsoft-update-catalog-site"></a>存取 Microsoft Update 目錄網站

1.  在 WSUS 系統管理主控台中，選取頂端伺服器節點或**更新**，然後在 [**動作**] 窗格中按一下 [匯**入更新**]。 瀏覽器視窗會在 Microsoft Update 目錄網站上開啟。

2.  若要存取這個網站上的更新，您必須安裝 Microsoft Update Catalog activeX 控制項。

3.  您可以流覽此網站以取得 Windows 修補程式和硬體驅動程式。 當您找到想要的專案時，請將其新增至您的購物籃。

4.  當您完成流覽時，請移至購物籃，然後按一下 [匯入] 以匯入您的更新。 若要下載更新而不匯入，請清除 [**直接匯入 Windows Server Update Services** ] 核取方塊。

從 Microsoft Update 目錄網站匯入的核准更新會在下次 WSUS 伺服器進行同步處理時下載。 從 Microsoft Update 目錄網站匯入時，不會下載它們。

請注意，您必須透過 WSUS 主控台存取 Microsoft Update 目錄網站，以確保以與 WSUS 相容的格式匯入更新。 如果您以手動方式存取 Microsoft Update 目錄網站，則您下載的任何更新都不會匯入到 WSUS 伺服器，而是以個別的 * 下載。MSU 檔案。 WSUS 目前不支援在 \*中匯入檔案的機制。MSU 格式。

如果您執行 [伺服器清理嚮導]，從已設定為 [未核准] 或 [已拒絕] 的 Microsoft Update 目錄匯入的更新，可能會從 WSUS 伺服器中移除。 如果已移除，則可以從 Microsoft Update 目錄重新匯入它們。

> [!NOTE]
> 您可以藉由執行 [WSUS 伺服器清理嚮導]，從已設定為 [未核准] 或 [已拒絕] 的 Microsoft Update 目錄中移除匯入的更新。 您可以重新匯入先前已透過 Microsoft Update 目錄從 WSUS 系統移除的更新。

## <a name="restricting-access-to-hotfixes"></a>限制對修補程式的存取
WSUS 系統管理員可能會考慮限制他們從 Microsoft Update 類別目錄網站下載的修復程式的存取權。 為了進行這項限制，請遵循下列步驟：

#### <a name="to-restrict-access-to-hotfixes"></a>限制對修補程式的存取

1.  在 [IIS 內容] vroot 上啟用 [Windows 驗證]。

    -   啟動 IIS 管理員。

    -   流覽至 [WSUS 系統管理] 網站底下的 [內容] 節點。

    -   在 [**內容] 首頁**窗格中，按兩下 [**驗證**] 選項。

    -   選取 [**匿名驗證**]，然後按一下右邊 [**動作**] 窗格中的 [**停**用]。

    -   選取 [ **Windows 驗證**]，然後按一下右邊 [**動作**] 窗格中的 [**啟用**]。

2.  為需要此修補程式的電腦建立 WSUS 目標群組，並將其新增至群組。 如需電腦和群組的詳細資訊，請參閱本指南中的[管理 Wsus 用戶端電腦和 wsus 電腦群組](managing-wsus-client-computers-and-wsus-computer-groups.md)和第[3.3 節。](../deploy/2-configure-wsus.md#23-configure-wsus-computer-groups)在 wsus 部署指南中設定步驟3：設定 wsus 的 wsus 電腦群組。

3.  下載此修補程式的檔。

4.  設定這些檔案的許可權，如此一來，只有那些電腦的機器帳戶可以讀取這些檔案。 您也必須允許 Network Service 帳戶完整存取檔案。

5.  為在步驟2中建立的 WSUS 目標群組核准此修補程式。

## <a name="importing-updates-in-different-languages"></a>匯入不同語言的更新
Microsoft Update Catalog 網站包含支援多種語言的更新。 請**務必符合**WSUS 伺服器支援的語言與這些更新所支援的語言。 如果 WSUS 伺服器不支援更新中包含的所有語言，更新就不會部署到用戶端電腦。 同樣地，如果支援多種語言的更新已下載到 WSUS 伺服器，但尚未部署到用戶端電腦，而且系統管理員取消選擇更新包含的其中一種語言，更新就不會部署到用戶端。
