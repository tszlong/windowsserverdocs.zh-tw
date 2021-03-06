---
title: 管理虛擬桌面
description: '瞭解如何在 MultiPoint 服務中管理虛擬桌面 (VDI) '
ms.topic: article
ms.assetid: fa9ac0ed-47cb-4811-91ff-4fcb62d7858b
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 58f1e86b6e6fe9e622d161c36f3eeec5179269be
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970575"
---
# <a name="manage-virtual-desktops"></a>管理虛擬桌面
單一電腦 VDI 可讓您設定每個*本機*MultiPoint 服務站，以連線至 hyper-v 虛擬機器中執行的 Windows 10 企業版客體作業系統， (VM) 與工作站相同的 MultiPoint 服務電腦上。 如果無法在 Windows Server 版本中安裝這些虛擬桌面站台，您可以使用應用程式來加以自訂。

## <a name="enable-the-virtual-desktop-feature"></a>啟用虛擬桌面功能

1.  開啟 MultiPoint 管理員，然後按一下 [虛擬桌面]**** 索引標籤。

2.  在 [VDI Tasks (VDI 工作)]**** 下，按一下 [建立虛擬桌面]****，然後瀏覽至您的 Windows 10 Enterprise .iso 或 VHD。

系統隨即重新啟動，但這可能需要幾分鐘的時間。

## <a name="create-a-virtual-desktop-template"></a>建立虛擬桌面範本

1.  開啟 MultiPoint 管理員，然後按一下 [虛擬桌面]**** 索引標籤。

2.  在 [VDI Tasks (VDI 工作)]**** 下，按一下 [建立虛擬桌面]****，然後瀏覽至您的 Windows 10 Enterprise .iso 或 VHD。

    如果使用 DVD，程式會自動尋找 Windows 10 Enterprise .wim 檔案。 否則，請按一下 [瀏覽]****，然後瀏覽至 Windows 10 Enterprise .iso 或 VHD。

    如有需要，您可以修改首碼。 它會預設為主機電腦的名稱。

    > [!NOTE]
    > 首碼是用來命名範本和虛擬桌面站台。 範本會以首碼 \-t 來命名。 虛擬桌面站台將會以首碼 \-*n* 來命名，其中 *n* 為站台識別碼。

4.  輸入將用來登入所有虛擬站台桌面 (從範本建立) 的本機系統管理員帳戶名稱和密碼，然後按一下 [確定]****。

    建立範本需要數分鐘才能完成。

    接下來，我們來了解如何自訂虛擬桌面範本。

    > [!NOTE]
    > 如果 Multipoint Server 已加入網域，對話方塊會填入額外欄位，以讓您指定從範本建立的虛擬機器是否應加入網域。

## <a name="import-a-virtual-desktop-template"></a>匯入虛擬桌面範本
如果您已在另一部 Multipoint Server 建立虛擬桌面範本，您可以使用下列步驟匯入該範本。

1.    開啟 MultiPoint 管理員，然後按一下 [虛擬桌面]**** 索引標籤。

2.    在 [VDI Tasks (VDI 工作)] 下，按一下 [Import Virtual desktop template (匯入虛擬桌面範本)]****。

3.    找出範本，並針對匯入的範本定義路徑和首碼。

## <a name="customize-the-virtual-desktop-template"></a>自訂虛擬桌面範本
建立虛擬桌面範本之後，您可以搭配應用程式、軟體更新加以自訂，並進行系統設定。

1. 開啟 MultiPoint 管理員，然後按一下 [虛擬桌面]**** 索引標籤。
2. 選擇虛擬桌面範本，然後按一下 [Customize virtual desktop template (自訂虛擬桌面範本)]****。
範本會在個別視窗中開啟，並反白顯示自訂虛擬範本最重要步驟的其他指示。 請詳讀這些指示。

## <a name="create-virtual-desktop-stations"></a>建立虛擬桌面工作站

1.  在站台模式中開啟 MultiPoint 管理員，然後按一下 [虛擬桌面]**** 索引標籤。

    > [!NOTE]
    > 如果 MultiPoint 服務系統未在站台模式下執行，則必須先重新啟動才能完成此程序。

2.  在左\-側窗格選取虛擬桌面範本。 它會命名為 <prefix – t>。

3.  在 [Template Tasks (範本工作)] 下，按一下 [Create virtual desktop stations (建立虛擬桌面站台)]****，然後按一下 [確定]****。

    虛擬桌面站台的建立程序需要幾分鐘時間。

    > [!NOTE]
    > 如果任何本機站台目前正連接到以工作階段為基礎的虛擬桌面，則您必須登出這些站台，以連接到新建立的虛擬桌面站台。

### <a name="validate-the-newly-created-customized-virtual-station-desktops"></a>驗證新建立的自訂虛擬站台桌面

若要驗證自訂虛擬站台桌面，請使用本機系統管理員帳戶或網域帳戶登入一或多個虛擬桌面站台後，再驗證以 VM 為基礎的新虛擬桌面是否能夠正常運作。

## <a name="disable-virtual-desktops"></a>停用虛擬桌面

停用虛擬桌面時，將會關閉 Hyper-V 功能。 系統會將所有使用者登出，並重新啟動。 系統重新啟動之後，會將所有虛擬站台指派給 MultiPoint 本機工作階段。

1. 在站台模式中開啟 MultiPoint 管理員，然後按一下 [虛擬桌面]**** 索引標籤。

2. 在 [VDI Tasks (VDI 工作)] 下，按一下 [Disable virtual desktops (停用虛擬桌面)]****。