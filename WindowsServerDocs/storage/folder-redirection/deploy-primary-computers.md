---
title: 部署資料夾重新導向和漫遊使用者設定檔的主要電腦
description: 如何啟用主要電腦支援，並為具有資料夾重新導向和漫遊使用者設定檔的使用者指定主要電腦。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: be2b41cf32e2020422c32415e2d8f4273eb09859
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394435"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>部署資料夾重新導向和漫遊使用者設定檔的主要電腦

>適用於：Windows 10、Windows 8、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012、Windows Server 2012 R2

本主題說明如何啟用主要電腦支援，並為使用者指定主要電腦。 這麼做可讓您控制哪些電腦使用資料夾重新導向和漫遊使用者設定檔。

> [!IMPORTANT]
> 啟用漫遊使用者設定檔的主要電腦支援時，請務必同時啟用主要電腦對資料夾重新導向的支援。 這會將檔和其他使用者檔案保留在使用者設定檔之外，協助設定檔保持在較小的狀態，且登入時間會保持快速。

## <a name="prerequisites"></a>必要條件

## <a name="software-requirements"></a>軟體需求

主要電腦支援具有下列需求：

- Active Directory Domain Services （AD DS）架構必須更新以包含新增的 Windows Server 2012 架構（安裝 Windows Server 2012 網域控制站會自動更新架構）。 如需有關更新 AD DS 架構的詳細資訊，請參閱[adprep 整合](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>)和執行[adprep](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>)。
- 用戶端電腦必須執行 Windows 10、Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。

> [!TIP]
> 雖然主要電腦支援需要資料夾重新導向和/或漫遊使用者設定檔，但如果您是第一次部署這些技術，最好先設定主要電腦支援，再啟用設定資料夾重新導向的 Gpo。漫遊使用者設定檔。 這樣可以防止使用者資料在啟用主要電腦支援之前被複製到非主要電腦。 如需設定資訊，請參閱[部署資料夾](deploy-folder-redirection.md)重新導向和[部署漫遊使用者設定檔](deploy-roaming-user-profiles.md)。

## <a name="step-1-designate-primary-computers-for-users"></a>步驟 1:指定使用者的主要電腦

部署主要電腦支援的第一個步驟是指定每個使用者的主要電腦。 若要這麼做，請使用 Active Directory 管理中心來取得相關電腦的辨別名稱，然後設定**PrimaryComputer**屬性。

> [!TIP]
> 若要使用 Windows PowerShell 來處理主要電腦，請參閱[深入探討深入探索 windows 8 主要電腦](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>)的 blog 文章。

以下說明如何指定使用者的主要電腦：

1. 在已安裝 Active Directory 系統管理工具的電腦上開啟伺服器管理員。
2. 在 [**工具**] 功能表上，選取 [ **Active Directory 系統管理中心**]。 [Active Directory 系統管理中心] 隨即顯示。
3. 流覽至適當網域中的 [**電腦**] 容器。
4. 在要指定為主電腦的電腦上按一下滑鼠右鍵，**然後選取 [** 內容]。
5. 在流覽窗格中，選取 [**擴充**功能]。
6. 選取 [**屬性編輯器**] 索引標籤、[流覽至**distinguishedName**]、[選取**視圖**]、以滑鼠右鍵按一下列出的值、選取 [**複製** **]、[確定]** ，然後選取 [**取消**]。
7. 流覽至適當網域中的 [**使用者**] 容器，以滑鼠右鍵按一下您要指派電腦的使用者，**然後選取 [** 內容]。
8. 在流覽窗格中，選取 [**擴充**功能]。
9. 選取 [**屬性編輯器**] 索引標籤，選取 [ **PrimaryComputer** ]，然後選取 [**編輯**]。 [多重字串值編輯器] 對話方塊隨即顯示。
10. 以滑鼠右鍵按一下文字方塊，選取 [**貼**上]，選取 [**新增**]，選取 **[確定]** ，然後再次選取 **[確定]** 。

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>步驟 2:（選擇性）在群組原則中啟用資料夾重新導向的主要電腦

下一步是選擇性地設定群組原則，以啟用資料夾重新導向的主要電腦支援。 這麼做可讓使用者的資料夾在指定為使用者主要電腦的電腦上重新導向，而不是在任何其他電腦上。 您可以依每部電腦或每個使用者的方式，控制資料夾重新導向的主要電腦。

以下是針對資料夾重新導向啟用主要電腦的方式：

1. 在 [群組原則管理] 中，以滑鼠右鍵按一下您在執行資料夾重新導向和/或漫遊使用者設定檔的初始設定時所建立的 GPO （例如，[資料夾重新導向**設定**] 或 [**漫遊使用者設定檔設定**]），然後選取 [**編輯**]。
2. 若要使用以電腦為基礎的群組原則來啟用主要電腦支援，請流覽至 [**電腦**設定]。 針對以使用者為基礎的群組原則，流覽至 [**使用者**設定]。
    - 以電腦為基礎的群組原則會將主要電腦處理套用至 GPO 套用的所有電腦，以影響電腦的所有使用者。
    - 以使用者為基礎的群組原則，將主要電腦處理套用至 GPO 套用的所有使用者帳戶，而影響使用者登入的所有電腦。
3. 在 [**電腦**設定] 或 [**使用者**設定] 底下，依序流覽至 [**原則**]**系統管理範本**、[**系統**] 和 [**資料夾**重新導向]。
4. 以滑鼠右鍵按一下 [**只重新導向主要電腦上的資料夾**]，然後選取 [**編輯**]。
5. 選取 [**已啟用**]，然後選取 **[確定]** 。

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>步驟 3：（選擇性）在群組原則中啟用漫遊使用者設定檔的主要電腦

下一步是選擇性地設定群組原則，以啟用漫遊使用者設定檔的主要電腦支援。 這麼做可讓使用者的設定檔在指定為使用者主要電腦的電腦上漫遊，而不是在任何其他電腦上。

以下說明如何啟用漫遊使用者設定檔的主要電腦：

1. 啟用資料夾重新導向的主要電腦支援（如果尚未這麼做）。<br>這會將檔和其他使用者檔案保留在使用者設定檔之外，協助設定檔保持在較小的狀態，且登入時間會保持快速。
2. 在群組原則管理 中，以滑鼠右鍵按一下您建立的 GPO （例如，資料夾重新導向**和 漫遊使用者設定檔設定**），然後選取 **編輯**。
3. 依序流覽至 [**電腦**設定]、[**原則**]、[**系統管理範本**]、[**系統**] 和 [**使用者設定檔**]。
4. 以滑鼠右鍵按一下 [**只下載主要電腦上的漫遊設定檔]，** 然後選取 [**編輯**]。
5. 選取 [**已啟用**]，然後選取 **[確定]** 。

## <a name="step-4-enable-the-gpo"></a>步驟 4：啟用 GPO

完成資料夾重新導向和漫遊使用者設定檔的設定後，如果您還沒有的話，請啟用 GPO。 這樣做可允許將它套用至受影響的使用者和電腦。

以下是啟用資料夾重新導向和/或漫遊使用者設定檔 Gpo 的方法：

1. 開啟群組原則管理
2. 以滑鼠右鍵按一下您建立的 Gpo，然後選取 [**啟用連結**]。 功能表項目旁邊應該會出現一個核取方塊。

## <a name="step-5-test-primary-computer-function"></a>步驟 5：測試主要電腦功能

若要測試主要電腦支援，請登入主要電腦，確認資料夾和設定檔已重新導向，然後登入非主要電腦，並確認資料夾和設定檔不會重新導向。

以下是測試主要電腦功能的方法：

1. 使用您已啟用資料夾重新導向和/或漫遊使用者設定檔的使用者帳戶，登入指定的主要電腦。
2. 如果使用者帳戶先前已登入電腦，請以系統管理員身分開啟 [Windows PowerShell 會話] 或 [命令提示字元] 視窗，輸入下列命令，然後在出現提示時登出，以確保最新的群組原則設定會套用至用戶端電腦：

    ```PowerShell
    Gpupdate /force
    ```

3. 開啟 [檔案總管]。
1. 以滑鼠右鍵按一下重新導向的資料夾（例如，文件庫中的 [我的文件] 資料夾），然後選取 [**屬性**]。
1. 選取 [**位置**] 索引標籤，並確認路徑顯示您指定的檔案共用，而不是本機路徑。 若要確認使用者設定檔是否為漫遊，請開啟 [**控制台**]，依序選取 [**系統及安全性**]、[**系統**]、[**高級系統設定**]、[使用者設定檔] 區段中的 [**設定**]，然後尋找[**類型**] 資料行中的漫遊。
1. 使用相同的使用者帳戶登入未指定為使用者主要電腦的電腦。
1. 重複步驟2–5，改為尋找本機路徑和**本機**配置檔案類型。

> [!NOTE]
> 如果在啟用主要電腦支援之前，已將資料夾重新導向到電腦上，除非在每個資料夾的資料夾重新導向原則設定中設定下列設定，否則資料夾會保持重新導向：**移除原則時，將資料夾重新導向回到本機的 userprofile 位置**。 同樣地，先前在特定電腦上漫遊的設定檔，將會在**類型**資料行中顯示**漫遊**;不過，[**狀態**] 欄將會顯示 [**本機**]。

## <a name="more-information"></a>詳細資訊

- [使用離線檔案部署資料夾重新導向](deploy-folder-redirection.md)
- [部署漫遊使用者設定檔](deploy-roaming-user-profiles.md)
- [資料夾重新導向、離線檔案和漫遊使用者設定檔總覽](folder-redirection-rup-overview.md)
- [深入探討稍微深入探索 Windows 8 主要電腦](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)