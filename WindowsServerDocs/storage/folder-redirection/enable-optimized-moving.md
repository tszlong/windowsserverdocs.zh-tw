---
title: 啟用已重新導向之資料夾的優化移動
description: 如何執行優化的重新導向資料夾移動到新的檔案共用。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: edf714bc0d6b39dbe7c5e800e953d7820fe9abc5
ms.sourcegitcommit: bfe9c5f7141f4f2343a4edf432856f07db1410aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75352603"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>啟用已重新導向之資料夾的優化移動

>適用于： Windows 10、Windows 8、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server （半年通道）

本主題描述如何執行優化的重新導向資料夾（資料夾重新導向）移至新的檔案共用。 如果您啟用此原則設定，當系統管理員移動裝載重新導向資料夾的檔案共用，並更新群組原則中重新導向資料夾的目標路徑時，就會直接在本機離線檔案快取中重新命名快取的內容，而不會有任何延遲或使用者可能遺失資料。

之前，系統管理員可以在群組原則中變更重新導向資料夾的目標路徑，並讓用戶端電腦在受影響的使用者下一次登入時複製檔案，而導致延遲登入。 或者，系統管理員可以移動檔案共用，並更新群組原則中重新導向資料夾的目標路徑。 不過，在用戶端電腦上于移動開始與第一次同步處理後的任何變更都會遺失。

## <a name="prerequisites"></a>先決條件

優化的移動具有下列需求：

- 必須設定資料夾重新導向。 如需詳細資訊，請參閱[使用離線檔案部署資料夾](deploy-folder-redirection.md)重新導向。
- 用戶端電腦必須執行 Windows 10、Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Server （半年通道）。

## <a name="step-1-enable-optimized-move-in-group-policy"></a>步驟1：啟用群組原則中的優化移動

若要優化資料夾重新導向資料的重新配置，請使用群組原則，以啟用在適當群組原則物件（GPO）的 [資料夾重新導向**伺服器路徑] 變更原則設定上，啟用優化的離線檔案內容移動**。 將此原則設定設為 [**停用**] 或 [**未設定**] 會導致用戶端將所有資料夾重新導向內容複寫到新位置，然後在伺服器路徑變更時，從舊位置刪除內容。

以下說明如何啟用已重新導向之資料夾的最佳移動：

1. 在群組原則管理 中，以滑鼠右鍵按一下您為資料夾重新導向設定（例如 資料夾重新導向**和 漫遊使用者設定檔設定**）所建立的 GPO，然後選取 **編輯**。
2. 在 [**使用者**設定] 下，依序流覽至 [**原則**]、**系統管理範本**、[**系統**] 和 [**資料夾**重新導向]。
3. 以滑鼠右鍵按一下 [在資料夾重新導向**伺服器路徑變更時，優化離線檔案快取中的內容移動**]，然後選取 [**編輯**]。
4. 選取 [**已啟用**]，然後選取 **[確定]** 。

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>步驟2：為重新導向的資料夾重新放置檔案共用

移動包含使用者重新導向之資料夾的檔案共用時，請務必採取預防措施，以確保資料夾已正確重新置放。

>[!IMPORTANT]
>如果使用者的檔案正在使用中，或在移動時未保留完整的檔案狀態，則當檔案透過網路複製、離線檔案產生的同步處理衝突，甚至是資料遺失時，使用者可能會遇到效能不佳的情況。

1. 事先通知使用者，主控其重新導向資料夾的伺服器將會變更，並建議他們執行下列動作：

      - 同步處理其離線檔案快取的內容，並解決任何衝突。
      - 開啟提升許可權的命令提示字元，輸入**GpUpdate/target： User/force**，然後登出再重新登入，以確保最新的群組原則設定會套用至用戶端電腦

        >[!NOTE]
        >根據預設，用戶端電腦每隔90分鐘會更新群組原則，因此，如果您允許用戶端電腦有足夠的時間來接收更新的原則，就不需要要求使用者使用**GpUpdate**。
2. 從伺服器中移除檔案共用，以確保檔案共用中沒有任何檔案正在使用中。 若要伺服器管理員，請在 [檔案和存放服務] 的 [**共用**] 頁面上，以滑鼠右鍵按一下適當的檔案共用，然後選取 [**移除**]。

    使用者將會使用離線檔案離線工作，直到移動完成，並從群組原則接收更新的資料夾重新導向設定。

3. 使用具有備份許可權的帳戶，使用可保留檔案時間戳記的方法（例如備份和還原公用程式），將檔案共用的內容移到新位置。 若要使用**Robocopy**命令，請開啟提升許可權的命令提示字元，然後輸入下列命令，其中 ```<Source>``` 是檔案共用的目前位置，而 ```<Destination>``` 是新的位置：

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >**Xcopy**命令不會保留所有檔案狀態。
4. 編輯資料夾重新導向原則設定，更新您要重新放置之每個重新導向資料夾的目的檔案夾位置。 如需詳細資訊，請參閱[使用離線檔案部署資料夾](deploy-folder-redirection.md)重新導向的步驟4。
5. 通知使用者裝載其重新導向資料夾的伺服器已變更，且應使用**GpUpdate/target： User/force**命令，然後登出再重新登入，以取得更新的設定並繼續適當的檔案同步處理。

    使用者至少應登入所有電腦一次，以確保資料會在每個離線檔案快取中正確地重新放置。

## <a name="more-information"></a>其他資訊

* [使用離線檔案部署資料夾重新導向](deploy-folder-redirection.md)
* [部署漫遊使用者設定檔](deploy-roaming-user-profiles.md)
* [資料夾重新導向、離線檔案和漫遊使用者設定檔總覽](folder-redirection-rup-overview.md)