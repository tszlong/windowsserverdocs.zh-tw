---
title: 部署資料夾重新導向及漫遊使用者設定檔的主要電腦
description: 如何啟用主要電腦支援，以及指定的使用者具有資料夾重新導向及漫遊使用者設定檔的主要電腦。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: b6e0a019297dbee557e284508a329001cac93bde
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812516"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>部署資料夾重新導向及漫遊使用者設定檔的主要電腦

>適用於：Windows 10、windows 8、windows、 Windows 8.1、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012、windows Server 2012 R2

本主題描述如何啟用主要電腦支援，並指定使用者的主要電腦。 這樣可讓您控制哪些電腦使用資料夾重新導向和漫遊使用者設定檔。

> [!IMPORTANT]
> 當啟用主要電腦支援漫遊使用者設定檔時，一律啟用主要電腦支援資料夾重新導向以及。 如此可讓文件和其他使用者檔案從使用者設定檔，以協助保持小型設定檔，及登入時間保持快速。

## <a name="prerequisites"></a>先決條件

## <a name="software-requirements"></a>軟體需求

主要電腦支援具有下列需求：

- 必須更新 Active Directory 網域服務 (AD DS) 結構描述，以包含新增 Windows Server 2012 結構描述 （安裝在 Windows Server 2012 網域控制站會自動更新結構描述）。 如需更新 AD DS 結構描述的相關資訊，請參閱 < [Adprep.exe 整合](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>)並[執行 Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>)。
- 用戶端電腦必須執行 Windows 10，Windows 8.1，Windows 8、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。

> [!TIP]
> 雖然主要電腦支援所需資料夾重新導向和/或漫遊使用者設定檔，如果您要部署這些技術，第一次，所以最好先設定才能啟用 Gpo 設定資料夾重新導向主要電腦支援和漫遊使用者設定檔。 這樣可以防止使用者資料在啟用主要電腦支援之前被複製到非主要電腦。 組態資訊，請參閱[部署資料夾重新導向](deploy-folder-redirection.md)並[部署漫遊使用者設定檔](deploy-roaming-user-profiles.md)。

## <a name="step-1-designate-primary-computers-for-users"></a>步驟 1：指定使用者的主要電腦

部署主要電腦支援的第一個步驟指定每個使用者的主要電腦。 若要這樣做，請取得相關電腦的辨別的名稱，然後設定中使用 Active Directory 系統管理中心**msDs PrimaryComputer**屬性。

> [!TIP]
> 若要使用 Windows PowerShell 使用的主要電腦，請參閱部落格文章[插入主要電腦的 Windows 8 的深入探究](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>)。

以下是如何指定使用者的主要電腦：

1. 在已安裝 Active Directory 系統管理工具的電腦上開啟伺服器管理員。
2. 在 **工具**功能表上，選取**Active Directory 系統管理中心**。 [Active Directory 系統管理中心] 隨即顯示。
3. 瀏覽至**電腦**適當網域中的容器。
4. 以滑鼠右鍵按一下您想要將指定為主要電腦，然後選取 電腦**屬性**。
5. 在 [導覽] 窗格中，選取**延伸模組**。
6. 選取 **屬性編輯器**索引標籤上，捲動到  **distinguishedName**，選取**檢視**，以滑鼠右鍵按一下列出的值，然後選取**複製**，選取  **確定**，然後選取**取消**。
7. 瀏覽至**使用者**容器中適當的網域，以滑鼠右鍵按一下您想要指派的電腦]，然後選取 [的使用者**屬性**。
8. 在 [導覽] 窗格中，選取**延伸模組**。
9. 選取 **屬性編輯器**索引標籤上，選取**msDs PrimaryComputer** ，然後選取**編輯**。 [多重字串值編輯器] 對話方塊隨即顯示。
10. 以滑鼠右鍵按一下 [文字] 方塊中，選取**貼上**，選取**新增**，選取 **[確定]** ，然後選取**確定**一次。

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>步驟 2：選擇性地啟用群組原則中的資料夾重新導向主要電腦

下一個步驟是選擇性地設定群組原則來啟用資料夾重新導向主要電腦支援。 這麼做可讓使用者的資料夾重新導向使用者的主要電腦，為指定的電腦上，但不是能在任何其他電腦。 您可以控制資料夾重新導向主要電腦上以每部電腦為基礎或以每個使用者為基礎。

若要啟用主要電腦的資料夾重新導向的方法如下：

1. 在 群組原則管理，以滑鼠右鍵按一下進行資料夾重新導向和/或漫遊使用者設定檔的初始組態時，您建立的 GPO (例如**資料夾重新導向設定**或**漫遊的使用者設定檔設定**)，然後選取**編輯**。
2. 若要啟用主要電腦支援使用電腦為基礎的群組原則，請瀏覽至**電腦設定**。 以使用者為基礎的群組原則，瀏覽**使用者設定**。
    - 電腦為基礎的群組原則會套用至要套用 GPO，會影響電腦的所有使用者的所有電腦的主要電腦處理。
    - 使用者的群組原則套用到所有要套用 GPO，會影響登入使用者的所有電腦的使用者帳戶的主要電腦處理。
3. 底下**電腦設定**或是**使用者設定**，瀏覽至**原則**，然後**系統管理範本**，則**系統**，然後**資料夾重新導向**。
4. 以滑鼠右鍵按一下**在主要電腦上的資料夾重新導向**，然後選取**編輯**。
5. 選取  **Enabled**，然後選取**確定**。

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>步驟 3：選擇性地啟用漫遊使用者設定檔群組原則中的 主要電腦

下一個步驟是選擇性地設定群組原則來啟用主要電腦支援漫遊使用者設定檔。 這麼做可讓使用者的漫遊使用者的主要電腦，為指定的電腦上，但不是能在任何其他電腦的設定檔。

以下是如何漫遊使用者設定檔啟用主要電腦：

1. 如果您還沒有這麼做，請啟用資料夾重新導向的主要電腦支援。<br>如此可讓文件和其他使用者檔案從使用者設定檔，以協助保持小型設定檔，及登入時間保持快速。
2. 在 群組原則管理，以滑鼠右鍵按一下您建立的 GPO (例如**資料夾重新導向和漫遊使用者設定檔設定**)，然後選取**編輯**。
3. 瀏覽至**電腦組態**，然後**原則**，然後**系統管理範本**，然後**系統**，然後**使用者設定檔**。
4. 以滑鼠右鍵按一下**下載主要電腦，使用漫遊設定檔**，然後選取**編輯**。
5. 選取  **Enabled**，然後選取**確定**。

## <a name="step-4-enable-the-gpo"></a>步驟 4：啟用 GPO

一旦您已完成設定資料夾重新導向和漫遊使用者設定檔，如果您尚未這麼做，請啟用 GPO。 這樣做可讓它套用至受影響的使用者和電腦。

以下是如何啟用資料夾重新導向和/或漫遊使用者設定檔 Gpo:

1. 開啟 群組原則管理
2. 以滑鼠右鍵按一下您所建立，然後選取 Gpo**啟用連結**。 核取方塊，應該會出現在功能表項目旁邊。

## <a name="step-5-test-primary-computer-function"></a>步驟 5：測試主要電腦函式

若要測試的主要電腦支援，登入主要電腦，請確認，則會重新導向的資料夾和設定檔，然後登入非主要電腦，並確認該資料夾和設定檔不會重新導向。

若要測試的主要電腦功能的方法如下：

1. 指定的主要電腦，您已啟用資料夾重新導向和/或漫遊使用者設定檔的使用者帳戶登入。
2. 如果電腦先前的使用者帳戶登入，開啟 Windows PowerShell 工作階段或命令提示字元視窗，以系統管理員身分輸入下列命令，然後登出以確保最新的群組原則設定會套用至出現提示時用戶端電腦：

    ```PowerShell
    Gpupdate /force
    ```

3. 開啟 [檔案總管]。
1. 以滑鼠右鍵按一下 [重新導向的資料夾 （比方說，是在文件庫中的我的文件] 資料夾），然後按**屬性**。
1. 選取 **位置**索引標籤，並確認路徑中顯示您指定而不是本機路徑的檔案共用。 若要確認，漫遊使用者設定檔，請開啟**控制台中**，選取**系統及安全性**，選取**System**，選取**進階系統設定**，選取**設定**使用者設定檔 區段，然後尋找**漫遊**中**型別**資料行。
1. 使用未指定為使用者的主要電腦的電腦相同的使用者帳戶登入。
1. 重複步驟 2-5，改為尋找本機路徑並**本機**設定檔類型。

> [!NOTE]
> 如果啟用主要電腦支援之前，在電腦上已重新導向資料夾，資料夾會保留在重新導向，除非在每個資料夾的資料夾重新導向原則設定中設定下列設定：**當原則移除時，將資料夾重新導向回本機使用者設定檔的位置**。 同樣地，先前已在特定電腦漫遊的設定檔會顯示**漫遊**中**型別**資料行; 不過，**狀態**資料行將會顯示**本機**。

## <a name="more-information"></a>詳細資訊

- [部署資料夾重新導向與離線檔案](deploy-folder-redirection.md)
- [部署漫遊使用者設定檔](deploy-roaming-user-profiles.md)
- [資料夾重新導向、 離線檔案和漫遊使用者設定檔的概觀](folder-redirection-rup-overview.md)
- [探究有點 Windows 8 中的主要電腦](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)