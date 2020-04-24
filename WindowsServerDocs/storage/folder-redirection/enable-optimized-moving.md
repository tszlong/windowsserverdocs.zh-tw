---
title: 啟用重新導向資料夾的最佳移動方式
description: 如何針對新檔案共用，執行重新導向資料夾的最佳移動方式。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: edf714bc0d6b39dbe7c5e800e953d7820fe9abc5
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "75352603"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>啟用重新導向資料夾的最佳移動方式

>適用於：Windows 10、Windows 8、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 和 Windows Server (半年通道)

本主題描述如何針對新檔案共用，執行重新導向資料夾的最佳移動方式。 如果您啟用此原則設定，當系統管理員移動裝載重新導向資料夾的檔案共用，並更新群組原則中重新導向資料夾的目標路徑時，就會直接在本機離線檔案快取中重新命名快取的內容，不會有任何延遲或使用者可能遺失資料的情況。

之前，系統管理員可以在群組原則中變更重新導向資料夾的目標路徑，並讓用戶端電腦在受影響的使用者下一次登入時複製檔案，而導致延遲登入。 現在，系統管理員可以移動檔案共用，並更新群組原則中重新導向資料夾的目標路徑。 不過，在開始移動和移動之後的第一次同步間，在用戶端電腦上所做的任何變更都會遺失。

## <a name="prerequisites"></a>必要條件

最佳移動方式具備下列需求：

- 必須設定資料夾重新導向。 如需細資訊，請參閱[使用離線檔案部署資料夾重新導向](deploy-folder-redirection.md)。
- 用戶端電腦必須執行 Windows 10、Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 或 Windows Servers (半年通道)。

## <a name="step-1-enable-optimized-move-in-group-policy"></a>步驟 1：啟用群組原則中的最佳移動方式

若要將資料夾重新導向資料的重新配置最佳化，請使用群組原則，為適當的群組原則物件 (GPO) 啟用**在資料夾重新導向伺服器路徑變更時啟用離線檔案快取內容的最佳移動方式** 原則設定。 將此原則設定設為 [停用]  或 [未設定]  會導致用戶端將所有資料夾重新導向內容複製到新位置，然後在伺服器路徑變更時，從舊位置刪除內容。

如何啟用重新導向資料夾的最佳移動方式：

1. 在 [群組原則管理] 中，以滑鼠右鍵按一下您為資料夾重新導向設定建立的 GPO (例如 [資料夾重新導向和漫遊使用者設定檔設定]  )，然後選取 [編輯]  。
2. 在 [使用者組態]  下，瀏覽至 [原則]  ，然後移至 [系統管理範本]  、[系統]  ，再瀏覽至 [資料夾重新導向]  。
3. 以滑鼠右鍵按一下 [在資料夾重新導向伺服器路徑變更時啟用離線檔案快取內容的最佳移動方式]  ，然後選取 [編輯]  。
4. 選取 [啟用]  ，然後選取 [確定]  。

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>步驟 2：為重新導向資料夾重新配置檔案共用

移動包含使用者重新導向資料夾的檔案共用時，請務必採取預防措施，確保資料夾已正確重新配置。

>[!IMPORTANT]
>如果使用者的檔案正在使用中，或在移動時未保留完整的檔案狀態，則在透過網路複製檔案、離線檔案產生的同步衝突時，使用者可能會遇到效能不佳，甚至資料遺失的情況。

1. 將裝載重新導向資料夾的伺服器變更的消息事先通知使用者，並建議他們執行下列動作：

      - 同步處理離線檔案快取的內容，並解決任何衝突。
      - 開啟提升權限的命令提示字元，輸入 **GpUpdate /Target:User /Force**，然後登出再重新登入，以確保最新的群組原則設定會套用至用戶端電腦

        >[!NOTE]
        >根據預設，用戶端電腦每隔 90 分鐘會更新群組原則，因此，如果您允許用戶端電腦有足夠的時間來接收更新的原則，就無需要求使用者使用 **GpUpdate**。
2. 從伺服器中移除檔案共用，以確保檔案共用中沒有任何檔案正在使用。 若要在伺服器管理員中這樣做，請在 [檔案和存放服務] 的 [共用]  頁面上，以滑鼠右鍵按一下適當的檔案共用，然後選取 [移除]  。

    在完成移動之前，使用者會使用離線檔案離線工作，並會從群組原則接收更新的資料夾重新導向設定。

3. 使用具有備份權限的帳戶，使用可保留檔案時間戳記的方法 (例如備份和還原公用程式)，將檔案共用的內容移到新位置。 若要使用 **Robocopy** 命令，請開啟提升權限的命令提示字元，然後輸入下列命令，其中 ```<Source>``` 是檔案共用的目前位置，而 ```<Destination>``` 是新位置：

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >**Xcopy** 命令不會保留所有檔案狀態。
4. 編輯資料夾重新導向原則設定，更新您要重新配置之每個重新導向資料夾的目標資料夾位置。 如需詳細資訊，請參閱[使用離線檔案部署資料夾重新導向](deploy-folder-redirection.md)的步驟 4。
5. 通知使用者裝載其重新導向資料夾的伺服器已變更，且應該使用 **GpUpdate /Target:User /Force** 命令，然後登出再重新登入，以取得更新的組庇並繼續適當的檔案同步作業。

    使用者至少應登入所有電腦一次，以確保資料會在每個離線檔案快取中正確地重新配置。

## <a name="more-information"></a>其他資訊

* [使用離線檔案部署資料夾重新導向](deploy-folder-redirection.md)
* [部署漫遊使用者設定檔](deploy-roaming-user-profiles.md)
* [資料夾重新導向、離線檔案及漫遊使用者設定檔概觀](folder-redirection-rup-overview.md)