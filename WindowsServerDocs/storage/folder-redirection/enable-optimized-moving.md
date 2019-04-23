---
title: 啟用最佳化的移動的資料夾重新導向
description: 如何執行重新導向資料夾的最佳化的移至新的檔案共用。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 98fd5d50645ad454204dcf9dabf58e97c246ab1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853989"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>啟用最佳化的移動的資料夾重新導向

>適用於：Windows 10，Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2、 Windows Server 2016

本主題描述如何執行的重新導向的資料夾 （資料夾重新導向） 最佳化的移至新的檔案共用。 如果您啟用這個原則設定，當系統管理員將裝載重新導向的資料夾的檔案共用，並更新重新導向的資料夾，在 群組原則的目標路徑時，快取的內容會只是重新命名在本機的離線檔案快取，而不需要任何延遲或使用者的潛在資料遺失。

先前，系統管理員無法變更目標路徑的重新導向的資料夾，在 群組原則，然後讓用戶端將檔案複製在受影響的使用者下次登入，導致延遲的登入。 或者，系統管理員無法移動的檔案共用，然後更新目標資料夾路徑的重新導向群組原則中。 不過，在本機用戶端電腦之間移動的首次同步處理做移動之後的任何變更會遺失。

## <a name="prerequisites"></a>必要條件

最佳化的移動具有下列需求：

- 資料夾重新導向必須設定。 如需詳細資訊，請參閱[部署資料夾重新導向與離線檔案](deploy-folder-redirection.md)。
- 用戶端電腦必須執行 Windows 10，Windows 8.1，Windows 8、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。

## <a name="step-1-enable-optimized-move-in-group-policy"></a>步驟 1：啟用群組原則中的最佳化的移動

若要最佳化的資料夾重新導向資料重新配置，請使用群組原則來啟用**最佳化的啟用移動的離線檔案快取內容的資料夾重新導向伺服器路徑變更**適當的群組原則的原則設定物件 (GPO)。 設定此原則設定可**已停用**或是**未設定**會導致將資料夾重新導向的所有內容都複製到新的位置，然後再刪除內容從舊的位置，如果用戶端伺服器路徑變更。

以下是如何啟用最佳化移動的資料夾重新導向：

1. 在 群組原則管理，以滑鼠右鍵按一下您建立的資料夾重新導向設定的 GPO (例如**資料夾重新導向和漫遊使用者設定檔設定**)，然後選取**編輯**。
2. 底下**使用者設定**，瀏覽至**原則**，然後**系統管理範本**，然後**系統**，然後**資料夾重新導向**。
3. 以滑鼠右鍵按一下**最佳化的啟用移動的離線檔案快取內容的資料夾重新導向伺服器路徑變更**，然後選取**編輯**。
4. 選取  **Enabled**，然後選取**確定**。

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>步驟 2：重新配置重新導向資料夾的檔案的共用

移動檔案共用，其中包含使用者的重新導向的資料夾，時，匯入到採取預防措施以確保重新正確放置資料夾。

>[!IMPORTANT]
>如果使用者的檔案中使用，或如果移動不會保留完整的檔案狀態，使用者可能會遇到效能不佳，透過網路、 離線檔案或甚至是資料遺失所產生的同步處理衝突會複製的檔案。

1. 事先通知使用者在裝載其重新導向的資料夾的伺服器將會變更，並建議他們執行下列動作：

      - 同步處理其離線檔案快取的內容，並解決任何衝突。
      - 開啟提升權限的命令提示字元中，輸入**GpUpdate /target: user /Force**，接著登出並登入後，以確保最新的群組原則設定會套用到用戶端電腦

        >[!NOTE]
        >根據預設，用戶端電腦更新群組原則的每隔 90 分鐘，因此如果您允許足夠的時間用戶端電腦接收更新的原則，您不需要請要求使用者使用**GpUpdate**。
2. 移除伺服器，以確保在檔案共用中的任何檔案正在使用中的檔案共用。 若要這麼做在 [伺服器管理員] 中，在**共用**頁面上的檔案和存放服務，請以滑鼠右鍵按一下適當的檔案共用，然後選取**移除**。

    使用者可離線使用離線檔案，直到移動完成，且它們會從群組原則接收更新的資料夾重新導向設定。

3. 使用含有備份的權限的帳戶，將檔案共用的內容移動到新的位置使用的方法，會保留檔案的時間戳記，例如備份和還原公用程式。 若要使用**Robocopy**命令，開啟提升權限的命令提示字元，然後輸入下列命令，其中```<Source>```是目前的檔案共用位置和```<Destination>```是新的位置：

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >**Xcopy**命令不會保留所有的檔案狀態。
4. 編輯資料夾重新導向原則設定，更新您想要重新放置每個重新導向資料夾的目標資料夾位置。 如需詳細資訊，請參閱步驟 4[部署資料夾重新導向與離線檔案](deploy-folder-redirection.md)。
5. 通知裝載其重新導向的資料夾的伺服器已變更，而且他們應該使用的使用者**GpUpdate /target: user /Force**命令，並接著登出，然後重新登入以取得更新的組態，並繼續執行適當的檔案同步處理。

    使用者應該登入的所有機器至少一次以確保資料在每個離線檔案快取中取得正確重新定位。

## <a name="more-information"></a>詳細資訊

* [部署資料夾重新導向與離線檔案](deploy-folder-redirection.md)
* [部署漫遊使用者設定檔](deploy-roaming-user-profiles.md)
* [資料夾重新導向、 離線檔案和漫遊使用者設定檔的概觀](folder-redirection-rup-overview.md)