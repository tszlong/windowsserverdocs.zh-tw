---
title: WSUS 和類別目錄網站
description: Windows Server Update Service (WSUS) 主題-如何藉由存取 Microsoft Update 類別目錄站台將 hotfix 匯入 WSUS
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 298df36b856cba97ec19126f77456785e5eb6f50
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810926"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS 和類別目錄網站

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

類別目錄站台是您可以從中匯入 hotfix 和硬體的驅動程式的 Microsoft 位置。

## <a name="the-microsoft-update-catalog-site"></a>Microsoft Update 類別目錄站台
若要將 hotfix 匯入 WSUS，您必須從 WSUS 電腦存取 Microsoft Update 類別目錄站台。 是 WSUS 伺服器、 已安裝 WSUS 管理主控台的任何電腦可用來從類別目錄站台匯入的 hotfix。 您必須登入電腦以系統管理員身分來匯入的 hotfix。

#### <a name="to-access-the-microsoft-update-catalog-site"></a>若要存取 Microsoft Update 類別目錄站台

1.  在 WSUS 系統管理主控台中，選取 最常發生的伺服器 節點或**更新**，然後在**動作**窗格中，按一下**匯入更新**。 Microsoft Update 類別目錄網站就會開啟瀏覽器視窗。

2.  若要存取此站台更新，您必須安裝 Microsoft Update Catalog activeX 控制項。

3.  您可以瀏覽此網站的 Windows hotfix 和硬體驅動程式。 當您發現您想的要時請將它們新增至您的購物籃。

4.  當您完成瀏覽時，請移至購物籃，然後按一下 匯入至匯入您的更新。 若要下載的更新不匯入，請清除**直接匯入 Windows Server Update Services**核取方塊。

下一次同步處理的 WSUS 伺服器時，會下載從 Microsoft Update 類別目錄網站匯入的已核准的更新。 它們不會在從 Microsoft Update 類別目錄網站匯入的時間下載。

請注意，您必須存取 Microsoft Update 類別目錄站台透過 WSUS 主控台，以確保在與 WSUS 相容的格式匯入更新。 如果您以手動方式存取 Microsoft Update 類別目錄網站，任何您所下載的更新不會匯入到 WSUS 伺服器，但改為下載個人身分 *。MSU 檔案。 WSUS 目前沒有支援的機制，用來匯入檔案中的\*。MSU 格式。

如果您執行 伺服器清理精靈，從 Microsoft Update Catalog 匯入已設定為未獲核准或已拒絕的更新可能會移除從 WSUS 伺服器。 如果移除它們，它們可以是從 Microsoft Update 類別目錄重新匯入。

> [!NOTE]
> 您可以移除從 Microsoft Update Catalog 匯入所執行的 WSUS 伺服器清理精靈設定為 [未核准或拒絕] 的更新。 您可以重新匯入先前已從您的 WSUS 系統，透過 Microsoft Update Catalog 的更新。

## <a name="restricting-access-to-hotfixes"></a>限制對 hotfix 的存取
WSUS 系統管理員可能會考慮它們已從 Microsoft Update 類別目錄網站下載的 hotfix 來限制存取。 若要進行這項限制，請遵循下列步驟：

#### <a name="to-restrict-access-to-hotfixes"></a>若要限制存取 hotfix

1.  啟用 IIS 內容的 vroot 上的 Windows 驗證。

    -   啟動 IIS 管理員。

    -   瀏覽至 WSUS 系統管理網站底下的 內容 節點。

    -   在 **內容首頁**窗格中按兩下**驗證**選項。

    -   選取 **匿名驗證**然後按一下**停用**中**動作**右邊的窗格。

    -   選取  **Windows 驗證**然後按一下**啟用**中**動作**右邊的窗格。

2.  建立電腦需要此 hotfix，WSUS 目標群組，並將它們新增至群組。 如需電腦及群組的詳細資訊，請參閱[管理 WSUS 用戶端電腦和 WSUS 電腦群組](managing-wsus-client-computers-and-wsus-computer-groups.md)在此指南中，與區段[3.3。設定 WSUS 電腦群組](../deploy/2-configure-wsus.md#23-configure-wsus-computer-groups)的步驟 3:設定 WSUS，WSUS 部署指南中。

3.  下載 hotfix 的檔案。

4.  設定這些檔案的權限，以便只有一部機器的機器帳戶可以讀取它們。 您也必須允許網路服務帳戶完整存取權的檔案。

5.  核准 WSUS 目標群組，在步驟 2 中建立的 hotfix。

## <a name="importing-updates-in-different-languages"></a>匯入不同語言的更新
Microsoft Update 類別目錄網站包括支援多種語言的更新。 非常**重要**來比對 WSUS 伺服器可使用這些更新所支援的語言支援的語言。 如果 WSUS 伺服器不支援更新中包含的所有語言，更新將不會部署到用戶端電腦。 同樣地，如果支援多國語言的更新已下載至 WSUS 伺服器，但尚未部署到用戶端電腦，並以系統管理員取消選取的語言之一包含更新，更新將不會部署到用戶端。
