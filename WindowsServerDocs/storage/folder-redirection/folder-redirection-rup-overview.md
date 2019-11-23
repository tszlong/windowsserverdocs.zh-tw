---
title: 資料夾重新導向、離線檔案及漫遊使用者設定檔概觀
description: 資料夾重新導向、離線檔案和漫遊使用者設定檔技術的總覽。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: ae1b23244f141cd0806ee14d3c40117ba72aeebb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402055"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>資料夾重新導向、離線檔案及漫遊使用者設定檔概觀

>適用于： Windows 10、Windows 8、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012、Windows Server 2012 R2

本主題討論資料夾重新導向、離線檔案（用戶端快取或 CSC）以及漫遊使用者設定檔（有時稱為 RUP）技術，包括新功能和尋找其他資訊的位置。

## <a name="technology-description"></a>技術說明

資料夾重新導向與離線檔案一併使用時，可將本機資料夾 (例如 \[文件\] 資料夾) 的路徑重新導向到網路位置，同時在本機快取該資料夾的內容，以提升速度及可用性。 您可以使用漫遊使用者設定檔將使用者設定檔重新導向到網路位置。 這些功能通常稱為 Intellimirror。

- [**資料夾**重新導向] 可讓使用者和系統管理員以手動方式或使用群組原則，將已知資料夾的路徑重新導向至新位置。 這個新位置可以是本機電腦上的資料夾，或是檔案共用上的目錄。 使用者與重新導向資料夾中檔案的互動方式，就像它們仍然存在本機磁碟機中一樣。 例如，您可以將通常儲存在本機磁碟機上的 [文件] 資料夾重新導向到網路位置。 接著，使用者可從網路上的任一電腦使用資料夾中的檔案。
- **離線檔案** 可讓使用者使用網路檔案，即使伺服器的網路連線無法使用或速度很慢也一樣。 在線上工作時，檔案存取效能是依據網路和伺服器速度而定。 當離線工作時，會以本機存取速度從 [離線檔案] 資料夾擷取檔案。 在下列狀況下，電腦會切換到離線模式：
  - **Always Offline**模式已啟用
  - 伺服器無法使用
  - 網路連線速度低於可設定的閾值
  - 使用者使用 Windows 檔案總管中的 [離線工作] 按鈕手動切換到離線模式
- **漫遊使用者設定檔** 會將使用者設定檔重新導向到檔案共用，讓使用者可以在多部電腦上接收相同的作業系統和應用程式設定。 當使用者使用以檔案共用設定為設定檔路徑的帳戶登入電腦時，會將使用者的設定檔下載到本機電腦，並與本機設定檔合併（如果有的話）。 當使用者登出電腦時，他們的設定檔本機複本 (包含所有的變更) 會合併到設定檔的伺服器複本中。 一般而言，網路系統管理員會啟用網域帳戶的漫遊使用者設定檔。

## <a name="practical-applications"></a>實際應用

系統管理員可以使用資料夾重新導向、離線檔案以及漫遊使用者設定檔來集中儲存使用者資料及設定，以提供使用者在離線或網路或伺服器中斷時存取其資料的能力。 某些特定的應用包含：

- 集中化用戶端電腦用於系統管理工作的資料，例如使用伺服器備份工具來備份使用者資料夾及設定。
- 讓使用者在網路或伺服器中斷時可以繼續存取網路檔案。
- 最佳化頻寬使用狀況，並針對分公司使用者存取位於離站公司伺服器上的檔案和資料夾增強使用經驗。
- 讓行動使用者在離線工作或透過低速網路工作時可以存取網路檔案。

## <a name="new-and-changed-functionality"></a>新功能和變更的功能

下表說明這個版本中可用的資料夾重新導向、離線檔案以及漫遊使用者設定檔的一些主要變更。

| 特色/功能 | 新功能或更新功能？ | 描述 |
| --- | --- | --- |
| 永遠離線模式 | 新的 | 永遠離線工作，即使透過高速網路連線連接時也一樣，提供較快的檔案存取速度及較低的頻寬使用量。 |
| 成本感知同步處理 | 新的 | 協助使用者在使用具有使用量限制的計量付費連線時，或在另一個提供者的網路上漫遊時，避免同步處理的高資料使用量成本。 |
| 主要電腦支援 | 新的 | 可讓您將資料夾重新導向、漫遊使用者設定檔或兩者限制為僅供使用者的主要電腦使用。 |

## <a name="always-offline-mode"></a>永遠離線模式

從 Windows 8 和 Windows Server 2012 開始，系統管理員可以將離線檔案的使用者體驗設定為永遠離線工作，即使是透過高速網路連線連接也一樣。 Windows 預設每小時在背景中執行一次同步處理，以更新離線檔案快取中的檔案。

### <a name="what-value-does-always-offline-mode-add"></a>什麼值一律會新增？

永遠離線模式提供下列優點：

- 使用者在存取重新導向資料夾 (例如 [文件] 資料夾) 中的檔案時，可感受到比較快的速度。
- 網路頻寬減少，降低昂貴的 WAN 或計量付費連線 (例如 4G 行動網路) 的成本。

### <a name="how-has-always-offline-mode-changed-things"></a>永遠離線模式如何變更？

在 Windows 8 之前，Windows Server 2012 之前，使用者會在線上和離線模式之間進行轉換（視網路可用性和條件而定），即使已啟用低速連結模式（也稱為慢速連線模式）並設定1毫秒延遲臨界值。

使用 Always Offline 模式時，當設定 [設定**低速連結模式]** 群組原則設定，而且 [**延遲**閾值] 參數設定為1毫秒時，電腦永遠不會轉換到線上模式。 根據預設，每隔 120 分鐘在背景執行一次同步處理，但是您可以使用 **Configure Background Sync** 群組原則設定來設定同步處理。

如需詳細資訊，請參閱 [Enable the Always Offline Mode to Provide Faster Access to Files](enable-always-offline.md)。

## <a name="cost-aware-synchronization"></a>成本感知同步處理

使用成本感知同步處理時，Windows 會在使用者使用計量付費網路連線（例如4G 行動網路）、訂閱者接近或超過其頻寬限制，或在其他提供者網路上漫遊時，停用背景同步處理。

> [!NOTE]
> 計量付費網路連線的來回網路延遲通常低於在 Windows 8、Windows Server 2019、Windows Server 2016 和 Windows Server 中轉換成離線（慢速連線）模式的預設35毫秒延遲值2012。 因此，這些連線通常會自動轉換成離線 (慢速連線) 模式。

### <a name="what-value-does-cost-aware-synchronization-add"></a>成本感知同步處理會加入哪些值？

成本感知同步處理可協助使用者在使用具有使用量限制的計量付費連線時，或在其他提供者網路上漫遊時，避免非預期的高資料使用成本。

### <a name="how-has-cost-aware-synchronization-changed-things"></a>成本感知同步處理如何變更？

在 Windows 8 和 Windows Server 2012 之前，在計量付費網路上使用離線檔案時，想要將費用降至最低的使用者必須使用行動網路提供者的工具來追蹤其資料使用量。 然後，使用者可以在漫遊、接近頻寬限制或超過限制時手動切換到離線模式。

使用成本感知同步處理時，Windows 會在計量付費連線時，自動追蹤漫遊和頻寬使用量限制。 當使用者在漫遊、接近頻寬限制或超過限制時，Windows 會切換到離線模式並防止執行所有的同步處理。 使用者仍可手動啟動同步處理，系統管理員可以覆寫特定使用者 (例如行政人員) 的成本感知同步處理。

如需詳細資訊，請參閱 [Enable Background File Synchronization on Metered Networks](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11))。

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>資料夾重新導向及漫遊使用者設定檔的主要電腦

您現在可以為每個網域使用者指定一組電腦，稱為主要電腦，讓您控制哪些電腦使用資料夾重新導向、漫遊使用者設定檔或兩者。 指定主要電腦是一個簡單且功能強大的方法，用來將使用者資料及設定與特定電腦或裝置產生關聯、簡化系統管理員的監督作業、改善資料安全性，以及協助保護使用者設定檔免於毀損。

### <a name="what-value-do-primary-computers-add"></a>主要電腦會新增什麼值？

指定使用者的主要電腦有四個主要的優點：

- 系統管理員可以指定使用者可使用哪些電腦存取重新導向的資料及設定。 例如，系統管理員可以選擇在使用者的桌上型電腦和膝上型電腦之間漫遊使用者資料和設定，並在該使用者登入任何其他電腦（例如會議室電腦）時，不漫遊資訊。
- 指定主要電腦可降低將個人或公司資料遺留在使用者所登入之電腦的安全性及隱私權風險。 例如，登入員工電腦以進行暫時存取的一般管理員不會留下任何個人或公司資料。
- 主要電腦可以讓系統管理員降低不當設定或是毀損設定檔的風險，這些風險可能源自在不同設定的系統之間漫遊，例如 x86 型及 x64 型電腦之間。
- 使用者在非主要電腦上第一次登入所需的時間量（例如伺服器）速度較快，因為不會下載使用者的漫遊使用者設定檔和/或重新導向的資料夾。 登出時間也會減少，因為使用者設定檔的變更不需要上傳到檔案共用。

### <a name="how-have-primary-computers-changed-things"></a>主要電腦如何變更專案？

若要限制下載私密使用者資料到主要電腦，資料夾重新導向及漫遊使用者設定檔技術會在使用者登入電腦時執行下列邏輯檢查：

1. Windows 作業系統會檢查新的群組原則設定（**僅下載主要電腦上的漫遊設定檔**，並**只重新導向主要電腦上的資料夾**），以判斷 Active Directory Domain Services （AD DS）中的「**電腦**」屬性是否應該影響漫遊使用者設定檔或套用資料夾重新導向的決定。
2. 如果這個原則設定啟用主要電腦支援，Windows 會確認 AD DS 結構描述支援 **msDS-Primary-Computer** 屬性。 若是如此，Windows 會判斷使用者登入的電腦是否被指定為該使用者的主要電腦，如下所示：
    1. 如果電腦是其中一個使用者的主要電腦，Windows 會套用漫遊使用者設定檔和資料夾重新導向設定。
    2. 如果電腦不是其中一個使用者的主要電腦，Windows 會載入使用者的快取本機設定檔（如果有的話），或建立新的本機設定檔。 Windows 也會依據先前套用的群組原則設定所指定的移除動作 (保存在本機資料夾重新導向設定中)，移除任何現有的重新導向資料夾。

如需詳細資訊，請參閱 [Deploy Primary Computers for Folder Redirection and Roaming User Profiles](deploy-primary-computers.md)。

## <a name="hardware-requirements"></a>硬體需求

資料夾重新導向、離線檔案以及漫遊使用者設定檔需要 x64 型或 x86 型電腦，而且在 ARM (WOA) 型電腦上 Windows 不支援這些功能。

## <a name="software-requirements"></a>軟體需求

若要指定主要電腦，您的環境必須符合下列需求：

- Active Directory Domain Services （AD DS）架構必須更新為包含 Windows Server 2012 架構和條件（安裝 Windows Server 2012 或更新版本的網域控制站會自動更新架構）。 如需有關升級 AD DS 架構的詳細資訊，請參閱[將網域控制站升級至 Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md)。
- 用戶端電腦必須執行 Windows 10、Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012，並加入您所管理的 Active Directory 網域。

## <a name="more-information"></a>更多資訊

如需其他相關資訊，請參閱下列資源。

| 內容類型 | 參考 |
| --- | --- |
| 產品評估 | [以可靠的檔案服務和儲存空間支援資訊工作者](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[離線檔案的新功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>)（windows 7 和 windows Server 2008 R2）<br>[Windows Vista 離線檔案的新功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Windows Vista 中離線檔案的變更](<https://technet.microsoft.com/library/2007.11.offline.aspx>)（TechNet 雜誌） |
| 部署 | [部署資料夾重新導向、離線檔案和漫遊使用者設定檔](deploy-folder-redirection.md)<br>[執行使用者資料集中化解決方案：資料夾重新導向和離線檔案技術驗證與部署](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[管理漫遊使用者資料部署指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[設定 Windows 7 電腦之新離線檔案功能的逐步指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[使用資料夾重新導向](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[執行資料夾](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>)重新導向（Windows Server 2003） |
| 工具及設定 | [MSDN 上的離線檔案](https://msdn.microsoft.com/library/cc296092.aspx)<br>[離線檔案群組原則參考](https://msdn.microsoft.com/library/ms878937.aspx)（Windows 2000） |
| 社群資源 | [檔案服務和儲存體論壇](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[您好，編寫腳本的專家！如何使用 Windows 中的離線檔案功能？](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[您好，編寫腳本的專家！如何啟用和停用離線檔案？](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>) |
| 相關技術|[Windows Server 中的身分識別與存取](../../identity/identity-and-access.md)<br>[Windows Server 的儲存空間](../storage.md)<br>[遠端存取和伺服器管理](../../remote/index.md) |