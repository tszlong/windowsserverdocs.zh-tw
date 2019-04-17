---
title: 資料夾重新導向、 離線檔案和漫遊使用者設定檔概觀 （英文)
description: 資料夾重新導向、 離線檔案和漫遊使用者設定檔的技術概觀。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4e3cf32cd718b906f16fc09901284d8520177df8
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233494"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>資料夾重新導向、 離線檔案和漫遊使用者設定檔概觀 （英文)

>適用於： Windows 10、 Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2、 Windows Server 2016

本主題討論資料夾重新導向、 離線檔案 （用戶端快取或 CSC） 和漫遊使用者設定檔 （有時稱為 RUP） 技術，包括的新功能及其他資訊的所在位置。

## <a name="technology-description"></a>技術描述

資料夾重新導向與離線檔案一併使用時，可將本機資料夾 (例如 \[文件\] 資料夾) 的路徑重新導向到網路位置，同時在本機快取該資料夾的內容，以提升速度及可用性。 您可以使用漫遊使用者設定檔將使用者設定檔重新導向到網路位置。 若要為稱為 Intellimirror 使用這些功能。

- **資料夾重新導向**可讓使用者與系統管理員可以將重新導向已知的資料夾路徑至新的位置，以手動方式或使用群組原則。 新的位置可以是本機電腦上的資料夾或檔案共用上的目錄。 使用者互動好像仍然存在於本機磁碟機上重新導向的資料夾中的檔案。 例如，您可以將文件] 資料夾，通常是儲存在本機磁碟機，網路位置重新導向。 資料夾中的檔案會再提供給使用者在網路上的任何電腦。
- **離線檔案**網路檔案可讓使用者即使伺服器的網路連線無法使用或速度過慢。 當線上工作時檔案存取效能會受網路及伺服器的速度。 離線工作時的檔案會擷取位於本機存取速度離線檔案資料夾中。 電腦會切換到離線模式時：
  - 已啟用**一律離線**模式
  - 伺服器已無法使用
  - 網路連線緩慢超過可設定的臨界值
  - 使用者手動會切換到離線模式使用 Windows 檔案總管中的 [**離線工作**] 按鈕
- **漫遊使用者設定檔**會重新導向使用者設定檔的檔案共用，讓使用者會收到相同的作業系統和多部電腦上的應用程式設定。 當使用者登入電腦所使用的帳戶，已設定為設定檔路徑的檔案共用與時、 使用者設定檔下載至本機電腦且合併與本機設定檔 （如果存在的話）。 當使用者登出電腦，包括任何變更，其設定檔的本機複本已合併之設定檔的伺服器複本。 通常是網路系統管理員啟用漫遊使用者設定檔上的網域帳戶。

## <a name="practical-applications"></a>實際應用

系統管理員可以使用資料夾重新導向、 離線檔案和漫遊使用者設定檔來集中管理的使用者資料和設定儲存空間和使用者提供存取其資料時離線或網路或伺服器中斷的情況下的能力。 某些特定的應用程式包括：

- 將集中管理工作，例如使用伺服器端的備份工具來備份使用者資料夾和設定的用戶端電腦的資料。
- 即使沒有網路或伺服器中斷，請讓使用者可繼續存取網路檔案。
- 最佳化的頻寬用量並提高中存取檔案及資料夾所主控的公司伺服器位於離站分公司的使用者體驗。
- 讓行動裝置使用者可同時使用離線或透過慢速網路存取網路檔案。

## <a name="new-and-changed-functionality"></a>新功能和變更的功能

下表說明一些資料夾重新導向、 離線檔案和漫遊使用者設定檔此版本中可用的主要變更。

|特色/功能|新功能或更新功能？|描述|
|---|---|---|
|永遠離線模式|新增|提供更快速存取檔案及較低頻寬使用量一律離線，即使透過高速網路連線。|
|成本感知的同步處理|新增|協助使用者避免收到高資料流量成本從或是其他提供者的網路上漫遊時使用的流量限制 metered 的連線同步處理。|
|主要的電腦支援|新增|可讓您限制使用資料夾重新導向、 漫遊使用者設定檔，或兩者都設為使用者的主要電腦。|

## <a name="always-offline-mode"></a>永遠離線模式

啟動與 Windows 8 和 Windows Server 2012，管理員可以設定為永遠離線工作，即使當所連接透過高速網路連線的離線檔案的使用者經驗。 Windows 更新來同步處理每小時在背景中預設的離線檔案快取中的檔案。

### <a name="what-value-does-always-offline-mode-add"></a>值永遠離線模式新增？

永遠離線模式有下列優點：

- 使用者體驗更快的存取權重新導向的資料夾，例如文件] 資料夾中的檔案。
- 為降低網路頻寬，會降低成本昂貴的 WAN 連線或 metered 例如 4 G 行動網路連線。

### <a name="how-has-always-offline-mode-changed-things"></a>如何一律離線模式已變更的事項？

在 Windows 8、 Windows Server 2012 之前使用者會轉換成線上和離線模式，視網路可用性與條件，即使緩慢連結模式 （也稱為慢速連線模式） 已啟用且設為 1 毫秒之間延遲臨界值。

使用一律離線模式時，電腦永不轉換成以線上模式時已**設定低速連結模式**] 群組原則設定，並**延遲**臨界值參數設為 1 毫秒。 變更會同步處理在背景中每隔 120 分鐘，根據預設，但是使用的**設定背景同步處理**群組原則設定的可設定同步處理。

如需詳細資訊，請參閱[啟用一律離線模式來提供更快的存取權的檔案](enable-always-offline.md)。

## <a name="cost-aware-synchronization"></a>成本感知的同步處理

使用成本感知的同步處理 Windows 會停用背景同步處理使用者是使用 metered 的網路連線，例如 4 G 行動網路和訂閱者是附近或超過其頻寬限制，或其他提供者的網路上漫遊時。

>[!NOTE]
>Metered 的網路連線通常會有的預設 35 毫秒延遲值轉換成離線 （慢速連線） 模式在 Windows 8、 Windows Server 2012 與 Windows Server 2016 比慢的來回網路延遲。 因此，這些連線通常轉換成離線 （慢速連線） 模式自動。

### <a name="what-value-does-cost-aware-synchronization-add"></a>成本感知的同步處理將新增的值為何？

成本感知的同步處理可協助使用者避免意外高資料流量成本或是其他提供者的網路上漫遊時使用 metered 的流量限制的連線。

### <a name="how-has-cost-aware-synchronization-changed-things"></a>如何成本感知的同步處理已變更的事項？

在 Windows 8 和 Windows Server 2012、 之前想要使用離線檔案 metered 的網路連線時將費用降至最低的使用者都追蹤其資料流量使用從行動裝置的網路提供者的工具。 使用者無法再手動切換成離線模式時他們所漫遊、 接近其頻寬限制，或超過其限制

使用成本感知的同步處理 Windows 自動追蹤漫遊和頻寬使用量限制在 metered 連線上。 當漫遊使用者、 接近其頻寬限制，或透過其限制、 Windows 會切換到離線模式和禁止所有同步處理。 使用者可以仍手動啟動同步處理，和系統管理員可以覆寫特定的使用者，例如高階主管的成本感知同步處理。

如需詳細資訊，請參閱[在 Metered 網路上啟用背景檔案同步](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11))。

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>主要電腦資料夾重新導向和漫遊使用者設定檔

您現在可以指定一組稱為 「 主要的電腦，每個可讓您控制哪些電腦會使用資料夾重新導向、 漫遊使用者設定檔或這兩者的網域使用者的電腦。 指定主要電腦是簡單和強大的方法來建立關聯的使用者資料和設定特定電腦或裝置，簡化管理員負責監督、 提升資料安全性和保護使用者設定檔從損毀。

### <a name="what-value-do-primary-computers-add"></a>主要的電腦加入值為何？

有四種來指定使用者的主要電腦的主要優點：

- 系統管理員可以指定哪些使用者可用來存取其重新導向的資料和設定的電腦。 例如，系統管理員可以選擇對漫遊使用者的桌面和筆記型電腦之間的使用者資料和設定和該使用者登入時任何其他電腦上，例如會議會議室電腦漫遊的資訊。
- 指定主要電腦減少的安全性和隱私權風險離開剩餘的個人或公司資料的使用者已經登入的電腦上。 例如，一般管理員登入的暫時存取員工的電腦不會留下任何個人或公司的資料。
- 主要的電腦啟用系統管理員可以降低風險的設定不正確或否則所得漫遊之間有不同的損毀設定檔系統，例如之間設定 x86 架構及 x64 型電腦。
- 使用者的漫遊使用者設定檔及 （或） 重新導向的資料夾就不會下載失敗原因在於更快的使用者的第一個登入非主要在電腦上，例如伺服器所需的時間長度。 登出時間是也減少，因為不需要使用者設定檔的變更可上傳到的檔案共用。

### <a name="how-have-primary-computers-changed-things"></a>如何主要電腦已變更的事項？

若要限制下載至主要電腦的私用的使用者資料，資料夾重新導向和漫遊使用者設定檔的技術時的使用者登入電腦執行下列邏輯檢查：

1. Windows 作業系統會檢查新群組原則設定 （**下載 （英文） 主要的電腦上的漫遊設定檔**和**主要的電腦上的重新導向資料夾**） 來判斷**型主要電腦**屬性在使用中Directory 網域服務 (AD DS) 應影響漫遊使用者設定檔或套用資料夾重新導向的決策。
2. 如果原則設定可讓支援主要的電腦，Windows 驗證的 AD DS 結構描述支援**型主要電腦**屬性。 若是如此，Windows 決定是否之電腦的使用者登入已指定為使用者的主要電腦，如下所示：
    1. 如果電腦是其中一個使用者的主要電腦、 Windows 適用於漫遊使用者設定檔和資料夾重新導向設定。
    2. 如果電腦不是其中一個使用者的主要電腦、 Windows 載入使用者的快取的本機設定檔，如果存在，或建立新的本機設定檔。 Windows 也會移除任何現有的重新導向的資料夾根據先前已套用的群組原則設定會保留在本機資料夾重新導向設定所指定的 [移除] 動作。

如需詳細資訊，請參閱[部署資料夾重新導向和漫遊使用者設定檔之主要電腦](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>硬體需求

資料夾重新導向、 離線檔案和漫遊使用者設定檔需要 x64 型或 x86 型電腦及它們不支援 Windows ARM WOA 型電腦上。

## <a name="software-requirements"></a>軟體需求

若要指定主要的電腦，您的環境必須符合下列需求：

- 必須更新 Active Directory 網域服務 (AD DS) 結構描述加入 Windows Server 2012 結構描述及條件 （自動安裝 Windows Server 2012 或更新版本的網域控制站更新結構描述）。 如需升級的 AD DS 結構描述的詳細資訊，請參閱[升級至 Windows Server 2016 的網域控制站](../../identity/ad-ds/deploy/upgrade-domain-controllers.md)。
- 用戶端電腦必須執行 Windows 10、 Windows 8.1、 Windows 8、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，並加入至您正在管理的 Active Directory 網域。

## <a name="more-information"></a>更多資訊

如需其他相關資訊，請參閱下列資源。

|內容類型|參考|
|---|---|
|產品評估|[支援使用可靠檔案服務及儲存的資訊工作者](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[離線檔案中的新功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>)（Windows 7 和 Windows Server 2008 R2）<br>[Windows Vista 的離線檔案的新功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[在 Windows Vista 離線檔案的變更](<https://technet.microsoft.com/library/2007.11.offline.aspx>)（TechNet 雜誌） （英文）|
|部署|[部署資料夾重新導向、 離線檔案和漫遊使用者設定檔](deploy-folder-redirection.md)<br>[實作使用者資料集中性解決方案： 資料夾重新導向及離線檔案技術驗證和部署](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[管理漫遊使用者資料部署指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[設定新離線適用於 Windows 7 的電腦逐步指南檔案的功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[使用資料夾重新導向](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[實作資料夾重新導向](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>)(Windows Server 2003)|
|工具及設定|[MSDN 上的離線檔案](https://msdn.microsoft.com/library/cc296092.aspx)<br>[離線檔案群組原則參考 （英文)](https://msdn.microsoft.com/library/ms878937.aspx)(Windows 2000)|
|社群資源|[檔案服務和儲存體論壇](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[吧、 Scripting Guy ！ 如何使用 Windows 的離線檔案功能？](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[吧、 Scripting Guy ！ 如何啟用和停用離線檔案？](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)|
相關技術|[身分識別與 Windows Server 中的存取](../../identity/identity-and-access.md)<br>[Windows Server 的儲存空間](../storage.md)<br>[遠端存取和伺服器管理](../../remote/index.md)|