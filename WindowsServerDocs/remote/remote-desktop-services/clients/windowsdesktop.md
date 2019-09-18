---
title: 開始使用 Windows 桌面用戶端
description: 關於 Windows 桌面用戶端的基本資訊。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/13/2019
ms.localizationpriority: medium
ms.openlocfilehash: c864ba0e51054a553bfd53f845bd4d1c9ff3c8ba
ms.sourcegitcommit: 61767c405da44507bd3433967543644e760b20aa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2019
ms.locfileid: "70988236"
---
# <a name="get-started-with-the-windows-desktop-client"></a>開始使用 Windows 桌面用戶端

>適用於：Windows 10 和 Windows 7

您可以使用適用於 Windows 桌面的遠端桌面用戶端，從不同的 Windows 裝置遠端存取 Windows 應用程式和桌面。

> [!NOTE]
> - 本文件不適用於 Windows 隨附的遠端桌面連線 (MSTSC) 用戶端。 其適用於新的遠端桌面 (MSRDC) 用戶端。
> - 此用戶端目前僅支援從 [Windows 虛擬桌面](https://aka.ms/wvd)存取遠端應用程式和桌面。
> - 想知道適用於 Windows 桌面用戶端的新版本嗎？ 查看 [Windows 桌面用戶端的新功能](windowsdesktop-whatsnew.md)

## <a name="install-the-client"></a>安裝用戶端

您目前可下載適用於 Windows 64 位元的用戶端。 當用戶端可供更多 Windows 版本使用時，我們就會更新此清單。

- [Windows 64 位元用戶端](https://go.microsoft.com/fwlink/?linkid=2068602)

您可以為目前使用者安裝用戶端，這不需要系統管理員權限，或者您的系統管理員可以安裝和設定用戶端，讓裝置上的所有使用者都可以存取它。

安裝後，您可藉由搜尋**遠端桌面**，從 [開始] 功能表啟動用戶端。

## <a name="feeds"></a>摘要

藉由訂閱系統管理員提供給您的摘要，取得您可存取的受控資源清單 (例如應用程式和桌面)。 當您訂閱時，資源會在您的本機電腦上變成可用。 Windows 桌面用戶端目前支援從 Windows 虛擬桌面發佈的資源。

### <a name="subscribe-to-a-feed"></a>訂閱摘要

1. 從用戶端的主頁面 (也稱為「連線中心」)，點選 [訂閱]  。
2. 出現提示時，使用您的使用者帳戶登入。
3. 資源將會出現於依工作區分組的連線中心。

您可以使用下列其中一種方法來啟動資源：

- 移至連線中心，然後按兩下資源來啟動它。
- 您也可以移至 [開始] 功能表並尋找具有工作區名稱的資料夾，或在搜尋列中輸入資源名稱。

### <a name="workspace-details"></a>工作區詳細資料

訂閱之後，您可以在 [詳細資料] 面板上檢視工作區的其他相關資訊：

- 工作區的名稱
- 用來訂閱的 URL 和使用者名稱
- 應用程式和桌面的數目
- 上次更新的日期/時間
- 上次工作的狀態

存取 [詳細資料] 面板：

1. 從連線中心，點選工作區旁邊的溢位功能表 ( **...** )。
2. 從下拉式功能表選取 [詳細資料]  。
3. [詳細資料] 面板會出現在用戶端的右側。

訂閱之後，工作區會定期自動更新。 根據系統管理員所做的變更，可能會新增、變更或移除資源。

您也可以從 [詳細資料] 面板選取 [立即更新]  ，視需要手動尋找資源的更新。

### <a name="unsubscribe-from-a-feed"></a>取消訂閱摘要

這一節將教導您如何取消訂閱摘要。 您可以取消訂閱，以使用不同的帳戶重新訂閱，或從系統中移除您的資源。

1. 從連線中心，點選工作區旁邊的溢位功能表 ( **...** )。
2. 從下拉式功能表選取 [取消訂閱]  。
3. 檢閱對話方塊，然後選取 [繼續]  。

## <a name="update-the-client"></a>更新用戶端

除非由系統管理員停用，否則您會在有新版用戶端可用時收到通知。 此通知可直接顯示於連線中心或 Windows 控制中心。 選取通知以開始更新程序。

您也可以手動搜尋用戶端的新更新：

1. 從連線中心，點選用戶端頂端命令列上的溢位功能表 ( **...** )。
2. 從下拉式功能表選取 [關於]  。
3. 點選 [檢查更新]  。
4. 如有可用的更新，請點選 [安裝更新]  以更新用戶端。

## <a name="providing-feedback"></a>提供意見反應

有功能建議或想要回報問題嗎？ 請使用[意見反應中樞](feedback-hub://?tabid=2&contextid=883)告訴我們，您也可從用戶端存取意見反應中樞：

1. 從連線中心，點選用戶端頂端命令列上的溢位功能表 ( **...** )。
2. 從下拉式功能表選取 [摘要]  以開啟 [意見反應中樞]。
