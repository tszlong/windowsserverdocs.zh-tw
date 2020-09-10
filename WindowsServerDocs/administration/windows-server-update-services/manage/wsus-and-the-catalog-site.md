---
title: WSUS 和類別目錄網站
description: Windows Server Update Service (WSUS) 主題-如何藉由存取 Microsoft Update 類別目錄網站，將修補程式匯入 WSUS
ms.topic: article
ms.assetid: f19a8659-5a96-4fdd-a052-29e4547fe51a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7e0d3c76e66275fe052d5d337dd30c67d7980638
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624275"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS 和類別目錄網站

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

目錄網站是 Microsoft 的位置，可讓您匯入修補程式和硬體驅動程式。

## <a name="the-microsoft-update-catalog-site"></a>Microsoft Update 目錄網站
為了將修補程式匯入 WSUS，您必須從 WSUS 電腦存取 Microsoft Update 目錄網站。 無論是 WSUS 伺服器，任何已安裝 WSUS 系統管理主控台的電腦，都可以用來從類別目錄網站匯入修補程式。 您必須以系統管理員身分登入電腦，才能匯入修補程式。

#### <a name="to-access-the-microsoft-update-catalog-site"></a>存取 Microsoft Update 目錄網站

1.  在 WSUS 系統管理主控台中，選取最上層伺服器節點或  **更新**，然後在 [ **動作** ] 窗格中按一下 [匯 **入更新**]。 瀏覽器視窗將會在 Microsoft Update 目錄網站開啟。

2.  若要存取此網站上的更新，您必須安裝 Microsoft Update Catalog activeX 控制項。

3.  您可以流覽此網站以取得 Windows 修復程式和硬體驅動程式。 當您找到想要的專案時，請將其新增至購物籃。

4.  當您完成流覽時，請移至購物籃，然後按一下 [匯入] 以匯入您的更新。 若要下載更新但不匯入，請清除 [ **直接匯入 Windows Server Update Services** ] 核取方塊。

從 Microsoft Update 目錄網站匯入的核准更新，會在下一次 WSUS 伺服器同步時下載。 從 Microsoft Update 目錄網站匯入時，不會下載它們。

請注意，您必須透過 WSUS 主控台存取 Microsoft Update 目錄網站，以確保以與 WSUS 相容的格式匯入更新。 如果您以手動方式存取 Microsoft Update 目錄網站，您下載的任何更新都不會匯入 WSUS 伺服器，而是以個別的 * 形式下載。MSU 檔案。 WSUS 目前並不支援在中匯入檔案的機制 \* 。MSU 格式。

如果您執行 [伺服器清理嚮導]，從已設定為 [未核准] 或 [已拒絕] 的 Microsoft Update 目錄匯入的更新，可能會從 WSUS 伺服器移除。 如果移除它們，就可以從 Microsoft Update 目錄重新匯入。

> [!NOTE]
> 您可以執行 [WSUS 伺服器清理嚮導]，從設定為 [未核准] 或 [已拒絕] 的 Microsoft Update 目錄中移除匯入的更新。 您可以重新匯入先前透過 Microsoft Update 目錄從 WSUS 系統移除的更新。

## <a name="restricting-access-to-hotfixes"></a>限制對修補程式的存取
WSUS 系統管理員可能會考慮限制其從 Microsoft Update 目錄網站下載之修補程式的存取權。 為了進行這項限制，請遵循下列步驟：

#### <a name="to-restrict-access-to-hotfixes"></a>限制對修補程式的存取

1.  啟用 IIS Content vroot 上的 Windows 驗證。

    -   啟動 IIS 管理員。

    -   流覽至 [WSUS 系統管理網站] 下的 [內容] 節點。

    -   在 [ **內容首頁** ] 窗格中，按兩下 [ **驗證** ] 選項。

    -   選取 [**匿名驗證**]，然後在右側的 [**動作**] 窗格中按一下 [**停**用]。

    -   選取 [ **Windows 驗證**]，然後在右側的 [**動作**] 窗格中按一下 [**啟用**]。

2.  為需要該修正程式的電腦建立 WSUS 目標群組，並將其新增至群組。 如需有關電腦和群組的詳細資訊，請參閱本指南中的 [管理 Wsus 用戶端電腦和 wsus 電腦群組](managing-wsus-client-computers-and-wsus-computer-groups.md) ，以及第 [3.3 節。](../deploy/2-configure-wsus.md#23-configure-wsus-computer-groups) 在 wsus 部署指南中設定步驟3：設定 wsus 的 wsus 電腦群組。

3.  下載此修正程式的檔案。

4.  設定這些檔案的許可權，如此一來，只有這些電腦的電腦帳戶可以讀取這些檔案。 您也必須允許 Network Service 帳戶完整存取檔案。

5.  為在步驟2中建立的 WSUS 目標群組核准此修正程式。

## <a name="importing-updates-in-different-languages"></a>匯入不同語言的更新
Microsoft Update Catalog 網站包含支援多種語言的更新。 請 **務必** 將 WSUS 伺服器支援的語言與這些更新所支援的語言相符。 如果 WSUS 伺服器不支援更新中包含的所有語言，更新將不會部署到用戶端電腦。 同樣地，如果支援多種語言的更新已下載至 WSUS 伺服器，但尚未部署到用戶端電腦，而且系統管理員取消選擇其中一個包含更新的語言，則更新將不會部署至用戶端。
