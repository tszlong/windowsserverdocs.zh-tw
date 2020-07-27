---
title: 部署資料夾重新導向及漫遊使用者設定檔的主要電腦
description: 如何啟用主要電腦支援，並為具有資料夾重新導向和漫遊使用者設定檔的使用者指定主要電腦。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 935d3ccf7de777a71d7c75179629b448dbb73a08
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966280"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>部署資料夾重新導向及漫遊使用者設定檔的主要電腦

>適用於：Windows 10、Windows 8、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 和 Windows Server 2012 R2

本主題說明如何啟用主要電腦支援，並為使用者指定主要電腦。 這麼做可讓您控制要使用資料夾重新導向和漫遊使用者設定檔的電腦。

> [!IMPORTANT]
> 啟用漫遊使用者設定檔的主要電腦支援時，請務必同時啟用對資料夾重新導向的主要電腦支援。 這會保留使用者設定檔中的文件和其他使用者檔案，協助設定檔保持在較小的狀態，且提供更快速的登入時間。

## <a name="prerequisites"></a>必要條件

## <a name="software-requirements"></a>軟體需求

主要電腦支援必須具備以下條件：

- 必須更新 Active Directory Domain Services (AD DS) 結構描述，以包含 Windows Server 2012 結構描述新增項目 (安裝 Windows Server 2012 網域控制站會自動更新結構描述)。 如需更新 AD DS 結構描述的相關資訊，請參閱 [Adprep.exe 整合](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>)及[執行 Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>)。
- 用戶端電腦必須執行 Windows 10、Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。

> [!TIP]
> 雖然主要電腦支援需要資料夾重新導向和/或漫遊使用者設定檔，但如果您是第一次部署這些技術，最好先設定主要電腦支援，再啟用設定資料夾重新導向和漫遊使用者設定檔的 GPO。 這樣可以防止使用者資料在啟用主要電腦支援之前被複製到非主要電腦。 如需組態詳細資訊，請參閱[部署資料夾重新導向](deploy-folder-redirection.md)和[部署漫遊使用者設定檔](deploy-roaming-user-profiles.md)。

## <a name="step-1-designate-primary-computers-for-users"></a>步驟 1：指定使用者的主要電腦

部署主要電腦支援的第一個步驟是指定每個使用者的主要電腦。 若要這麼做，請使用 Active Directory 系統管理中心來取得相關電腦的辨別名稱，然後設定 **msDs-PrimaryComputer** 屬性。

> [!TIP]
> 若要使用 Windows PowerShell 處理主要電腦，請參閱部落格文章[深入探索 Windows 8 主要電腦](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>)。

如何為使用者指定主要電腦：

1. 在已安裝 Active Directory 系統管理工具的電腦上開啟伺服器管理員。
2. 在 [工具]  功能表上，選取 [Active Directory 系統管理中心]  。 [Active Directory 系統管理中心] 隨即顯示。
3. 瀏覽至適當網域中的 [電腦]  容器。
4. 在您想指定為主要電腦的電腦上按一下滑鼠右鍵，接著選取 [屬性]  。
5. 在瀏覽窗格中，選取 [擴充功能]  。
6. 選取 [屬性編輯器]  索引標籤，並瀏覽至 **distinguishedNam**，選取 [檢視]  ，再以滑鼠右鍵按一下列出的值，依序選取 [複製]  、[確定]  ，然後選取 [取消]  。
7. 瀏覽到適當網域中的 [使用者]  容器，在想要指派電腦的使用者上按一下滑鼠右鍵，然後按一下 [內容]  。
8. 在瀏覽窗格中，選取 [擴充功能]  。
9. 依序選取 [屬性編輯器]  索引標籤和 [msDs-PrimaryComputer]  ；然後選取 [編輯]  。 [多重字串值編輯器] 對話方塊隨即顯示。
10. 以滑鼠右鍵按一下文字方塊，依序選取 [貼上]  、[新增]  、[確定]  ，然後再次選取 [確定]  。

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>步驟 2：選擇性地在群組原則中啟用資料夾重新導向的主要電腦

下一步是選擇性地設定群組原則，以啟用資料夾重新導向的主要電腦支援。 這麼做可讓使用者的資料夾在指定為使用者主要電腦的電腦上重新導向，而不是在任何其他電腦上重新導向。 您可以依每部電腦或每個使用者，控制資料夾重新導向的主要電腦。

如何啟用資料夾重新導向的主要電腦：

1. 在 [群組原則管理] 中，以滑鼠右鍵按一下在執行資料夾重新導向和/或漫遊使用者設定檔的初始組態時建立的 GPO (例如 [資料夾重新導向設定]  或 [漫遊使用者設定檔設定]  )，然後選取 [編輯]  。
2. 若要使用電腦型群組原則來啟用主要電腦支援，請瀏覽至 [電腦組態]  。 若為使用者型的群組原則，請瀏覽至 [使用者組態]  。
    - 電腦型的群組原則會將主要電腦處理套用至所有套用 GPO 的電腦，才能影響電腦的所有使用者。
    - 電腦型的群組原則會將主要電腦處理套用至所有套用 GPO 的使用者帳戶，才能影響使用者登入的電腦。
3. 在 [電腦組態]  或 [使用者組態]  下，瀏覽至 [原則]  ，然後移至 [系統管理範本]  、[系統]  ，再移至 [資料夾重新導向]  。
4. 以滑鼠右鍵按一下 [僅重新導向主要電腦上的資料夾]  ，然後選取 [編輯]  。
5. 選取 [啟用]  ，然後選取 [確定]  。

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>步驟 3：選擇性地在群組原則中啟用漫遊使用者設定檔的主要電腦

下一步是選擇性地設定群組原則，以啟用漫遊使用者設定檔的主要電腦支援。 這麼做可讓使用者的設定檔在指定為使用者主要電腦的電腦上漫遊，而不是在任何其他電腦上漫遊。

如何啟用漫遊使用者設定檔的主要電腦：

1. 啟用資料夾重新導向的主要電腦支援 (如果尚未啟用)。<br>這會保留使用者設定檔中的文件和其他使用者檔案，協助設定檔保持在較小的狀態，且提供更快速的登入時間。
2. 在 [群組原則管理] 中，以滑鼠右鍵按一下您建立的 GPO (例如，[資料夾重新導向和漫遊使用者設定檔設定]  )，然後選取 [編輯]  。
3. 依序瀏覽至 [電腦組態]  、[原則]  、[系統管理範本]  、[系統]  ，然後是 [使用者設定檔]  。
4. 以滑鼠右鍵按一下 [僅下載主要電腦上的漫遊設定檔]  ，然後選取 [編輯]  。
5. 選取 [啟用]  ，然後選取 [確定]  。

## <a name="step-4-enable-the-gpo"></a>步驟 4：啟用 GPO

完成資料夾重新導向和漫遊使用者設定檔的設定後，請啟用 GPO (如果尚未啟用)。 這樣可允許系統將 GPO 套用至受影響的使用者和電腦。

如何啟用資料夾重新導向和/或漫遊使用者設定檔 GPO：

1. 開啟 [群組原則管理]
2. 以滑鼠右鍵按一下您建立的 GPO，然後選取 [啟用連結]  。 功能表項目旁應該會顯示核取方塊。

## <a name="step-5-test-primary-computer-function"></a>步驟 5：測試主要電腦功能

若要測試主要電腦支援，請登入主要電腦，確認資料夾和設定檔已重新導向，然後登入非主要電腦，並確認資料夾和設定檔並未重新導向。

如何測試主要電腦功能：

1. 使用已啟用資料夾重新導向和/或漫遊使用者設定檔的使用者帳戶，登入指定的主要電腦。
2. 如果使用者帳戶先前已登入電腦，請以系統管理員身分開啟 [Windows PowerShell 工作階段] 或 [命令提示字元] 視窗，輸入下列命令，然後在出現提示時登出，以確保最新的群組原則設定會套用至用戶端電腦：

    ```PowerShell
    Gpupdate /force
    ```

3. 開啟 [檔案總管]。
1. 以滑鼠右鍵按一下重新導向資料夾 (例如，文件庫中的 [我的文件] 資料夾)，然後選取 [屬性]  。
1. 選取 [位置]  索引標籤，並確認路徑顯示的是您指定的檔案共用，而不是本機路徑。 若要確認使用者設定檔正在漫遊，請開啟 [控制台]  ，依序選取 [系統及安全性]  、[系統]  、[進階系統設定]  ，選取 [使用者設定檔] 區段中的 [設定]  ，然後在 [類型]  欄位中尋找 [漫遊]  。
1. 使用相同的使用者帳戶登入未指定為使用者主要電腦的電腦。
1. 重複步驟 2–5，改為尋找本機路徑和**本機**路徑檔案類型。

> [!NOTE]
> 如果在啟用主要電腦支援之前，已在電腦上將資料夾重新導向，除非在每個資料夾的資料夾重新導向原則設定中設定了下列設定，否則資料夾仍然會保持重新導向：**當原則移除時，將資料夾重新導回使用者設定檔位置**。 同樣地，先前在特定電腦上漫遊的設定檔，將會在**類型**欄位中顯示**漫遊**；不過，[狀態]  欄位將會顯示 **本機**。

## <a name="more-information"></a>其他資訊

- [使用離線檔案部署資料夾重新導向](deploy-folder-redirection.md)
- [部署漫遊使用者設定檔](deploy-roaming-user-profiles.md)
- [資料夾重新導向、離線檔案及漫遊使用者設定檔概觀](folder-redirection-rup-overview.md)
- [深入探索 Windows 8 主要電腦](/archive/blogs/askds/digging-a-little-deeper-into-windows-8-primary-computer)
