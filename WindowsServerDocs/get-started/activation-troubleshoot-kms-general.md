---
title: 對 KMS 進行疑難排解的指導方針
description: 提供 KMS 服務的相關資訊，並建議用於排解啟用問題的工具和方法
ms.topic: troubleshooting
ms.date: 9/24/2019
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: b31f357089ed54ed5f350657979740e7fd58651a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941806"
---
# <a name="guidelines-for-troubleshooting-the-key-management-service-kms"></a>金鑰管理服務 (KMS) 疑難排解的指導方針

在其部署過程中，許多企業客戶都會設定金鑰管理服務 (KMS)，以便在其環境中進行 Windows 啟用。 設定 KMS 主機是很簡單的程序，其後，KMS 用戶端即可探索主機並嘗試自行啟用。 但如果該程序無法運作，該如何處理？ 您接下來該怎麼做？ 本文將逐步說明您在排解問題時所需的資源。 如需事件記錄檔項目和 Slmgr.vbs 指令碼的詳細資訊，請參閱[大量啟用技術參考](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502529(v=ws.11))。

## <a name="kms-overview"></a>KMS 概觀

我們先快速複習一下 KMS 啟用。 KMS 是一種用戶端-伺服器模型。 就概念而言，它類似於 DHCP。 KMS 並不會應用戶端的要求對其交付 IP 位址，而是會啟用產品啟用。 KMS 也是一種更新模型，用戶端會依據此模型定期嘗試重新啟用。 KMS 有兩個角色：*KMS 主機*和 *KMS 用戶端*。

- **KMS 主機**會執行啟用服務，並啟用環境中的啟用。 若要設定 KMS 主機，您必須從大量授權服務中心 (VLSC) 安裝 KMS 金鑰，然後啟用服務。
- **KMS 用戶端**是部署在環境中且必須啟用的 Windows 作業系統。 KMS 用戶端可執行任何使用大量啟用的 Windows 版本。 KMS 用戶端會獲得預先安裝的金鑰，名為一般大量授權金鑰 (GVLK) 或 KMS 用戶端安裝金鑰。 取得 GVLK 後，即可讓系統成為 KMS 用戶端。 KMS 用戶端會使用 DNS SRV 記錄 (_vlmcs._tcp) 來識別 KMS 主機。 然後，用戶端會自動嘗試探索並使用此服務，以自行啟用。 在 30 天的預設寬限期內，用戶端會以兩小時的間隔嘗試啟用。 啟用之後，KMS 用戶端會每七天一次的間隔嘗試更新其啟用。

從疑難排解的觀點來看，您可能必須同時查看這兩端 (主機和用戶端)，以判斷發生什麼狀況。

## <a name="kms-host"></a>KMS 主機

KMS 主機上有兩個需要檢查的區域。 首先，必須檢查主機軟體授權服務的狀態。 其次，必須檢查事件檢視器中是否有授權或啟用的相關事件。

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs 和軟體授權服務

若要查看軟體授權服務的詳細輸出，請開啟提升權限的 [命令提示字元] 視窗，並在命令提示字元中輸入 **slmgr.vbs /dlv**。 下列螢幕擷取畫面顯示 Microsoft 內的其中一個 KMS 主機執行此命令的結果。

![KMS 主機的 Slmgr 輸出](./media/ee939272.kms_slmgr_output(en-us,technet.10).png)

疑難排解最重要的欄位如下。 視要解決的問題而定，您應尋找的內容可能有所不同。

- **版本資訊**。 **slmgr.vbs /dlv** 輸出的上方會顯示軟體授權服務版本。 這可能有助於確認是否已安裝服務的最新版本。 以 Windows Server 2003 為例，其 KMS 服務的更新支援不同的 KMS 主機金鑰。 這項資料可用來評估版本是否為最新的，以及是否支援您嘗試安裝的 KMS 主機金鑰。 如需這些更新的詳細資訊，請參閱 [Windows Vista 和 Windows Server 2008 已有可用的更新可擴充至 Windows 7 和 Windows Server 2008 R2 的 KMS 啟用支援](https://support.microsoft.com/help/968912/an-update-is-available-for-windows-vista-and-for-windows-server-2008-t)。
- [名稱]。 這表示 KMS 主機系統上安裝的 Windows 版本。 如果您在新增或變更 KMS 主機金鑰時遇到問題 (例如，為了確認作業系統版本支援該金鑰)，這可能會是疑難排解中的重要資訊。
- [說明]。 您可以在此處看到已安裝的金鑰。 使用此欄位可確認先前用來啟用服務的金鑰，以及這是否為您已部署的 KMS 用戶端所適用的正確金鑰。
- **授權狀態**。 這是 KMS 主機系統的狀態。 其值應為**已授權**。 若為任何其他值則表示有問題，您可能必須重新啟用主機。
- **目前計數**。 顯示的計數會介於 **0** 與 **50** 之間。 此計數是累計的 (在作業系統之間)，表示在 30 天的期間內嘗試啟用的有效系統數目。

  如果計數為 **0**，表示服務最近已啟用，或沒有有效的用戶端連線至 KMS 主機。

  無論環境中有多少有效系統，計數都不會增加到 **50** 以上。 這是因為其計數設定為只會快取 KMS 用戶端所傳回之授權原則上限的兩倍。 當日的原則上限由 Windows 用戶端作業系統所設定，必須在 KMS 主機的計數達到 **25** 或更高時，才會自行啟用。 因此，KMS 主機的最高計數是 2 x 25 或50。 請注意，在僅包含 Windows Server KMS 用戶端的環境中，KMS 主機的計數上限將是 **10**。 這是因為 Windows Server 版本的閾值為 **5** (2 x 5，或 10)。

  與此計數有關的常見問題是，環境中有已啟用的 KMS 主機和足夠的用戶端，但計數並未增長到一以上。 核心問題是已部署的用戶端映像未正確設定 (**sysprep /generalize**)，且系統沒有唯一的用戶端電腦識別碼 (CMID)。 如需詳細資訊，請參閱 [KMS 用戶端](#kms-client)和[當您將新的 Windows Vista 或 Windows 7 用戶端電腦新增至網路時，KMS 的目前計數並未增加](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista)。 我們一名支援升級工程師也針對此問題發表了部落格文章：[KMS 主機用戶端計數因 CMID 重複而未增加](/archive/blogs/askcore/kms-host-client-count-not-increasing-due-to-duplicate-cmids)。

  計數未增加的另一個原因，是環境中有太多 KMS 主機，而計數分配給所有的主機。
- **接聽連接埠**。 與 KMS 的通訊會使用匿名 RPC。 根據預設，用戶端會使用 1688 TCP 連接埠連線至 KMS 主機。 請確定您的 KMS 用戶端與 KMS 主機之間已開啟此連接埠。 您可以變更或設定 KMS 主機上的連接埠。 在通訊期間，KMS 主機會將連接埠指定傳送給 KMS 用戶端。 如果您變更 KMS 用戶端上的連接埠，當該用戶端連線到主機時，就會覆寫該連接埠指定。

常有人會問到 **slmgr.vbs /dlv** 輸出的「累計要求」部分。 一般而言，這項資料對於疑難排解並沒有幫助。 KMS 主機會對每個嘗試啟用或重新啟用的 KMS 用戶端持續記錄其狀態。 失敗的要求表示 KMS 主機不支援的 KMS 用戶端。 例如，如果 Windows 7 KMS 用戶端嘗試對使用 Windows Vista KMS 金鑰啟用的 KMS 主機進行啟用，該啟用將會失敗。 「具有授權狀態的要求」行會說明所有可能的授權狀態，包括過去和現在的狀態。 從疑難排解的觀點來看，只有在計數未如預期增加時，此資料才有相關性。 在這種情況下，您應該會發現失敗的要求數目逐漸增加。 這表示您應檢查用來啟用 KMS 主機系統的產品金鑰。 同時請注意，只有在您重新安裝 KMS 主機系統時，累計要求值才會重設。

### <a name="useful-kms-host-events"></a>有用的 KMS 主機事件

#### <a name="event-id-12290"></a>事件識別碼 12290

當 KMS 用戶端連線至主機以進行啟用時，KMS 主機會記錄事件識別碼 12290。 事件識別碼 12290 提供了大量資訊，您可以利用這些資訊來了解與主機連線的用戶端類型，以及失敗的發生原因。 事件識別碼 12290 項目的下列區段來自於 KMS 主機的「金鑰管理服務」事件記錄檔。

![KMS 12290 事件](./media/ee939272.kms_12290_event(en-us,technet.10).png)

事件詳細資料包含下列資訊：

- **啟用所需的最小計數**。 KMS 用戶端報告，KMS 主機上的計數必須達到 **5**，才能啟用。 這表示這是 Windows Server 作業系統，但並未指出特定版本。 如果您的用戶端未啟用，請確定主機上有足夠的計數。
- **用戶端電腦識別碼 (CMID)** 。 這是每個系統上的唯一值。 如果此值不是唯一的，這是因為未正確備妥映像以供發佈 (**sysprep /generalize**)。 此問題在 KMS 主機上的具體徵狀為計數不會增加，即使環境中有足夠的用戶端也一樣。 如需詳細資訊，請參閱[當您將新的 Windows Vista 或 Windows 7 用戶端電腦新增至網路時，KMS 的目前計數並未增加](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista)。
- **授權狀態和狀態到期時間**。 這是用戶端目前的授權狀態。 這有助於您區分第一次嘗試啟用的用戶端與嘗試重新啟用的用戶端。 時間項目會指出，在沒有變更的情況下，用戶端還會處於該狀態多長的時間。

如果您嘗試對用戶端進行疑難排解，但在 KMS 主機上找不到對應的事件識別碼 12290，表示該用戶端並未連線至 KMS 主機。 以下是事件識別碼 12290 項目不存在的部分原因：

- 發生網路中斷的狀況。
- 主機未解析或未在 DNS 中註冊。
- 防火牆封鎖 TCP 1688。
   該連接埠有可能在環境中的許多位置遭到封鎖，包括 KMS 主機系統本身。 KMS 主機依預設會有 KMS 的防火牆例外，但不會自動啟用。 您必須開啟該例外。
- 事件記錄檔已滿。

KMS 用戶端會記錄兩個對應的事件，即事件識別碼 12288 和事件識別碼 12289。 如需這些事件的詳細資訊，請參閱 [KMS 用戶端](#kms-client)一節。

#### <a name="event-id-12293"></a>事件識別碼 12293

另一個應在 KMS 主機上尋找的相關事件，是事件識別碼 12293。 此事件表示主機未在 DNS 中發佈必要的記錄。 這種情況已知會導致失敗，您應在設定主機*之後*和部署用戶端*之前*確認是否有此情況。 如需 DNS 問題的詳細資訊，請參閱 [KMS 和 DNS 問題的常見疑難排解程序](common-troubleshooting-procedures-kms-dns.md)。

## <a name="kms-client"></a>KMS 用戶端

在用戶端上，您可以使用相同的工具 (Slmgr 和事件檢視器) 對啟用進行疑難排解。

### <a name="slmgrvbs-and-the-software-licensing-service"></a>Slmgr.vbs 和軟體授權服務

若要查看軟體授權服務的詳細輸出，請開啟提升權限的 [命令提示字元] 視窗，並在命令提示字元中輸入 **slmgr.vbs /dlv**。 下列螢幕擷取畫面顯示 Microsoft 內的其中一個 KMS 主機執行此命令的結果。

![KMS 用戶端的 Slmgr 輸出](./media/ee939272.kms_client_slmgr_output(en-us,technet.10).png)

下列清單包含疑難排解最重要的欄位。 視要解決的問題而定，您應尋找的內容可能有所不同。

- [名稱]。 此值是 KMS 用戶端系統上安裝的 Windows 版本。 使用此欄位可確認您嘗試啟用的 Windows 版本可以使用 KMS。 例如，我們的技術支援人員曾發現客戶嘗試在未使用大量啟用的 Windows 版本 (例如 Windows Vista Ultimate) 上安裝 KMS 用戶端安裝金鑰的事件。
- [說明]。 此值會顯示已安裝的金鑰。 VOLUME_KMSCLIENT 表示已安裝 KMS 用戶端安裝金鑰 (或 GVLK) (大量授權媒體的預設組態)，且此系統會自動嘗試使用 KMS 主機進行啟用。 如果您在此處看到其他內容 (例如 MAK)，則必須重新安裝 GVLK，以將此系統設定為 KMS 用戶端。 您可以使用 **slmgr.vbs /ipk &lt;*GVLK*&gt;** (如 [KMS 用戶端安裝金鑰](kmsclientkeys.md)中所說明)，或使用大量啟用管理工具 (VAMT)，以手動方式安裝金鑰。 如需取得和使用 VAMT 的相關資訊，請參閱[大量啟用管理工具 (VAMT) 技術參考](/windows/deployment/volume-activation/volume-activation-management-tool)。
- **部分產品金鑰**。 在 [名稱] 欄位中，您可以使用這項資訊來判斷此電腦上是否已安裝正確的 KMS 用戶端安裝金鑰 (也就是說，金鑰是否符合 KMS 用戶端上安裝的作業系統)。 根據預設，在使用大量授權服務中心 (VLSC) 入口網站的媒體建置的系統上，會有正確的金鑰。 在某些情況下，客戶可能會先使用多重啟用金鑰 (MAK) 啟用，直到環境中有足夠的系統可支援 KMS 啟用。 這些系統必須安裝 KMS 用戶端安裝金鑰，才能從 MAK 轉換至 KMS。 請使用 VAMT 來安裝此金鑰，並確實套用正確的金鑰。
- **授權狀態**。 此值會顯示 KMS 用戶端系統的狀態。 若為使用 KMS 啟用的系統，此值應為**已授權**。 若為任何其他值，就可能有問題。 例如，如果 KMS 主機正常運作，且 KMS 用戶端未啟用 (例如，它處於**寬限期**狀態)，則表示可能有某些問題導致用戶端無法連線到主機系統 (例如防火牆問題、網路中斷，或類似的因素)。
- **用戶端電腦識別碼 (CMID)** 。 每個 KMS 用戶端都應有唯一的 CMID。 如同 [KMS 主機](#kms-host)一節提到的，與計數有關的常見問題是，環境中有已啟用的 KMS 主機和足夠的用戶端，但計數並未增長到 **1** 以上。 如需詳細資訊，請參閱[當您將新的 Windows Vista 或 Windows 7 用戶端電腦新增至網路時，KMS 的目前計數並未增加](https://support.microsoft.com/help/929829/the-kms-current-count-does-not-increase-when-you-add-new-windows-vista)。
- **來自 DNS 的 KMS 機器名稱**。 此值會顯示用戶端成功用於啟用的 KMS 主機的 FQDN，以及用於通訊的 TCP 連接埠。
- **KMS 主機快取**。 最後一個值會顯示是否啟用快取。 依預設會加以啟用。 這表示，KMS 用戶端會快取它用於啟用的 KMS 主機名稱，且在需要重新啟用時會直接與此主機通訊 (而不是查詢 DNS)。 如果用戶端無法連線到快取的 KMS 主機，則會查詢 DNS 以探索新的 KMS 主機。

### <a name="useful-kms-client-events"></a>有用的 KMS 用戶端事件

#### <a name="event-id-12288-and-event-id-12289"></a>事件識別碼 12288 和事件識別碼 12289

當 KMS 用戶端成功啟用或重新啟用時，用戶端會記錄兩個事件：事件識別碼 12288 和事件識別碼 12289。 事件識別碼 12288 項目的下列區段來自於 KMS 用戶端的「金鑰管理服務」事件記錄檔。

![KMS 用戶端事件識別碼 12288](./media/ee939272.client_12288(en-us,technet.10).png)

如果您只看到事件識別碼 12288 (沒有對應的事件識別碼 12289)，表示 KMS 用戶端無法連線到 KMS 主機、KMS 主機沒有回應，或用戶端未收到回應。 在此情況下，請確認 KMS 主機可供探索，且 KMS 用戶端可與其連線。

事件識別碼 12288 中最具相關性的資訊，是 [資訊] 區段中的資料。 例如，此區段會顯示用戶端目前的狀態，以及用戶端在嘗試啟用時所使用的 FQDN 和 TCP 連接埠。 您可以使用 FQDN，對 KMS 主機上的計數未增加的狀況進行疑難排解。 例如，如果有太多 KMS 主機可供用戶端使用 (包括合法或惡意系統)，則計數可能會分配給所有的主機。

不成功的啟用不一定表示用戶端具有 12288 事件，而沒有 12289 事件。 失敗的啟用或重新啟用也可能同時有這兩個事件。 在此情況下，您必須檢查第二個事件，以確認失敗的原因。

![KMS 用戶端事件識別碼 12289](./media/ee939272.client_12289(en-us,technet.10).png)

事件識別碼 12289 的 [資訊] 區段提供下列資訊：

- **啟用旗標**。 此值會指出啟用是成功 (**1**) 還是失敗 (**0**)。
- **KMS 主機上的目前計數**。 此值會反映 KMS 主機上在用戶端嘗試啟用時的計數值。 如果啟用失敗，可能是因為此用戶端作業系統的計數不足，或環境中沒有足夠的系統可產生計數。

## <a name="what-does-support-ask-for"></a>支援有何要求？

如果您必須呼叫支援人員以對啟用進行疑難排解，支援工程師通常會要求您提供下列資訊：

- KMS 主機和 KMS 用戶端系統的 **Slmgr.vbs /dlv** 輸出。 無論您使用 wscript 還是 cscript 來執行命令，都可以使用 Ctrl+C 來複製輸出，然後將其貼到 [記事本] 中，並傳送給支援連絡人。
- KMS 主機的事件記錄檔 (金鑰管理服務記錄) 和 KMS 用戶端系統的事件記錄檔 (應用程式記錄)

## <a name="additional-references"></a>其他參考資料
- [詢問核心小組：啟用次數](/archive/blogs/askcore/kms-host-client-count-not-increasing-due-to-duplicate-cmids)
