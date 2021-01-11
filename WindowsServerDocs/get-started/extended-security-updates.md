---
title: Windows Server 2008 和 2008 R2 延伸安全性更新
description: 瞭解如何在其支援週期結束後，使用 Windows Server 2008 和 2008 R2 延伸安全性更新 (ESU)。
ms.mktglfcycl: manage
author: iainfoulds
ms.author: daveba
ms.topic: how-to
ms.localizationpriority: high
ms.date: 02/21/2020
ms.openlocfilehash: eeb1c011973b0b0d63ea93f838f144eee7defda0
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949264"
---
# <a name="how-to-use-windows-server-2008-and-2008-r2-extended-security-updates-esu"></a>如何使用 Windows Server 2008 和 2008 R2 延伸安全性更新 (ESU)

>適用於：Windows Server 2008 和 Windows Server 2008 R2

Windows Server 2008 和 Windows Server 2008 R2 支援週期已於 2020 年 1 月 14 日到期。 Windows Server 長期維護通道 (LTSC) 至少有十年的支援，五年的主要支援，以及五年的延伸支援。 這種支援包括定期的安全性更新。

結束支援也表示安全性更新的結束。 此案例可能會導致安全性或相容性問題，並讓商務應用程式面臨風險。 Microsoft 建議您[升級到目前版本的 Windows Server](modernize-windows-server-2008.md)，以獲得最先進的安全性、效能和創新。

如果您尚未升級伺服器，下列選項將有助於在轉換期間保護您的應用程式和資料：

* 將現有的 Windows Server 2008 和 2008 R2 工作負載依目前的方式遷移至 Azure 虛擬機器 (VM)。
  * 遷移到 Azure 會自動提供額外三年延伸安全性更新(ESU)。 延伸安全性更新在 Azure VM 成本上不會額外收費，而且不需要額外的設定。
* 請為您的伺服器購買延伸安全性更新訂閱，並保持受保護狀態，直到您準備好升級至更新的 Windows Server 版本。
  * 在支援生命週期日期結束之後，最多可提供三年的更新。

在為期三年的延伸更新之後，我們便會停止 Windows Server 2008 和 2008 R2 的更新。 建議您盡快將 Windows Server 的版本更新為較新版本。

## <a name="what-are-extended-security-updates-for-windows-server"></a>什麼是 Windows Server 的延伸安全性更新？

適用於 Windows Server 的延伸安全性更新 (ESU) 包含安全性更新和公告，等級為 *非常重要* 和 *重要*，自 2020 年 1月 14 日起最多三年。 延伸安全性更新不包含下列內容：

* 新功能
* 客戶要求的非安全性 Hotfix
* 設計變更要求

如需詳細資訊，請參閱[延伸安全性更新常見問題集](https://www.microsoft.com/cloud-platform/extended-security-updates)。

## <a name="how-to-use-extended-security-updates"></a>使用延伸安全性更新的方法

如果您在 Azure 中執行 Windows Server 2008 或 2008 R2 VM，這些 VM 會自動啟用延伸安全性更新。 您不需要執行任何設定，也不需額外付費即可使用 Azure VM 的延伸安全性更新。 如果設定為接收更新，則延伸安全性更新會自動傳遞至 Azure VM。

> [!NOTE]
> Microsoft.ClassicCompute VM 需要額外設定來進行延伸安全性更新部署，因為其無法存取 [Azure Instance Metadata Service](/azure/virtual-machines/windows/instance-metadata-service)，這會決定延伸安全性更新的資格。 請連絡 [Microsoft 支援服務](https://support.microsoft.com/contactus?PID=17336)尋求協助。

針對其他環境 (例如內部部署 VM 或實體伺服器)，您必須手動要求並設定延伸安全性更新。 您可以透過大量授權方案 (例如 Enterprise 合約 (EA)、Enterprise 合約訂用帳戶 (EAS)、教育版解決方案註冊 (EES) 或伺服器和雲端註冊 (SCE)) 來購買延伸安全性更新。

若您已購買延伸安全性更新，則可以使用下列其中一種方法來取得金鑰：

* 如果您想要從 Azure 入口網站取得延伸安全性更新金鑰，則可以[在 Azure 入口網站中註冊延伸安全性更新](#register-for-extended-security-updates-on-azure-portal)。
* 您也可以[登入 Microsoft 大量授權服務中心](#sign-in-to-the-microsoft-volume-licensing-service-center)來取得金鑰，而不需要使用 Azure 入口網站。

### <a name="register-for-extended-security-updates-on-azure-portal"></a>在 Azure 入口網站上註冊延伸安全性更新

若要在非 Azure VM 上使用延伸安全性更新，請建立多次啟用金鑰 (MAK)，並將此金鑰套用至 Windows Server 2008 和 2008 R2 電腦。 MAK 金鑰可讓 Windows Update 伺服器知道您可以繼續接收安全性更新。 即使您只使用內部部署電腦，也能使用 Azure 入口網站來註冊延伸安全性更新及管理這些金鑰。

> [!NOTE]
> 如果您正在 Azure VM 上執行 Windows Server 2008 和 2008 R2，則不需要註冊延伸安全性更新。 對於其他環境 (例如內部部署 VM 或實體伺服器)，請先[購買延伸安全性更新](https://www.microsoft.com/licensing/how-to-buy/how-to-buy)，再嘗試註冊並使用該更新。

若要為 VM 註冊延伸安全性更新並建立金鑰，請開啟 Azure 入口網站，並遵循下列指示：

1. 登入 [Azure 入口網站](https://portal.azure.com/)。
2. 在 Azure 入口網站頂端的 [搜尋] 方塊中，搜尋並選取 [延伸安全性更新]  。

    ![搜尋 Azure 入口網站中的延伸安全性更新](media/extended-security-updates/esu-portal-search.png)

    如果您之前尚未使用過延伸安全性更新，請先選取 [+ 建立]  來建立延伸安全性更新資源。 否則，請從清單中選取您的資源。

3. 在 [註冊延伸服務更新]  底下，選取 [開始使用]  。

    ![Azure 入口網站中的延伸安全性更新入門](media/extended-security-updates/get-started-with-esu.png)

4. 若要建立您的第一個金鑰，請選取 [取得金鑰]  。

    ![選擇在 Azure 入口網站中建立金鑰](media/extended-security-updates/get-key.png)

    您需要與帳戶相關聯的 Azure 訂用帳戶，才能建立延伸安全性更新資源和金鑰。 如果您沒有與帳戶相關聯的 Azure 訂用帳戶，請以不同的使用者帳戶登入，或在 Azure 入口網站中建立 Azure 訂用帳戶。

    Azure 訂用帳戶也必須獲派「參與者」角色，安全性更新才能正常運作。 若要檢查角色，請在搜尋方塊中輸入「訂用帳戶」。 您會看到一個資料表，並於訂用帳戶識別碼和名稱旁顯示您的角色。

    如果您不是參與者，則可以請訂用帳戶擁有者變更您的角色。 若要找出訂用帳戶的擁有者，請移至上一段所述的角色資料表，然後選取您訂用帳戶的名稱。 接下來，移至頁面左側的功能表，然後選取 [存取控制 (IAM)]   > [角色指派]  ，並尋找資料表的 [擁有者] 區段。

5. 如果您看到有頁面指出「註冊以取得多次啟用金鑰」，這表示您需要先要求私人預覽的存取權，才能使用延伸安全性更新。 如果您沒有看到此頁面，請跳到步驟 6。

   若要要求存取權，請選取 [加入私人預覽]  。 隨即會開啟電子郵件訊息視窗。 這封電子郵件是您對產品小組所提出的存取要求。

    請在要求中包含下列資訊：

    * 客戶名稱
    * Azure 訂用帳戶識別碼
    * 合約編號 (適用於 ESU)
    * ESU 伺服器數目

    當您完成時，請傳送電子郵件。

    該小組會檢閱您在要求電子郵件中提供的資訊。 如果一切正確，其便會將您新增至核准清單。

    如果該小組未核准您的要求，您便會看到下列錯誤：

    [在命名空間 'Microsoft.WindowsESU' 中找不到資源類型]()

6. 在 [Azure 詳細資料]  底下，選取您的 Azure 訂用帳戶、資源群組和金鑰的位置。

    在 [註冊詳細資料]  底下，輸入下列資訊：

    | 設定             | 值 |
    |---------------------|-------|
    | 索引鍵名稱            | 您金鑰的顯示名稱，例如 *Agreement01*。 |
    | 合約編號    | 大量授權合約管理系統所產生的合約編號，或 Enterprise 合約程式的 MSLicense。 |
    | 電腦數目 | 選擇您想要使用此金鑰安裝延伸安全性更新的電腦數目。 |
    | 作業系統    | 選擇要搭配使用此金鑰的作業系統，例如 Windows Server 2008 或 Windows Server 2008 R2。 |

    準備好時，請選取 [檢閱 + 註冊]  。

    >[!NOTE]
    >請確定您已在全域篩選中選取您用來加入私人預覽的 Azure 訂用帳戶。 選取 Azure 入口網站功能區中的 [篩選]  按鈕，以檢查您的全域訂用帳戶篩選。
    >
    > ![已選取 [篩選] 按鈕的 Azure 入口網站功能區影像](media/azure-ribbon-filter.png)

7. 驗證成功之後，就會顯示新登錄資源的選擇摘要。 如有需要，請更正任何驗證錯誤，或更新您的設定選擇。 Azure [使用規定](https://azure.microsoft.com/support/legal/)和[隱私權原則](https://privacy.microsoft.com/privacystatement)皆可使用。

    核取此方塊，以確認您有合格的電腦，且金鑰僅會在您的組織內使用：

    ![確認金鑰僅會由您的組織使用](media/extended-security-updates/confirm-key-usage.png)

    準備好時，請選取 [建立]  來產生 MAK。

延伸安全性更新註冊現在可與您的電腦搭配使用。 建立的金鑰應該套用到您想要保持符合安全性更新的 Windows Server 2008 和 2008 R2 電腦。

### <a name="sign-in-to-the-microsoft-volume-licensing-service-center"></a>登入 Microsoft 大量授權服務中心

如果您沒有 Azure 入口網站的存取權，則可以使用大量授權服務中心來檢視及下載啟用金鑰。

若要從大量授權服務中心取得金鑰：

1. 移至[大量授權服務中心頁面](https://www.microsoft.com/vlsc)，然後使用 Azure 認證來登入。

2. 選取 [授權]   > [關聯性摘要]   > [授權識別碼]   > [產品金鑰]  。

若要深入了解如何為合格的 Windows 裝置取得延伸安全性更新，請參閱[我們的技術社群貼文](https://techcommunity.microsoft.com/t5/windows-it-pro-blog/obtaining-extended-security-updates-for-eligible-windows-devices/ba-p/1167091#)。

## <a name="download-and-apply-extended-security-updates"></a>下載並套用延伸安全性更新

Windows Server 延伸安全性更新的傳遞、下載及應用程式，與現有的部署程序並無不同。 透過延伸安全性更新提供的更新，僅適用於 *安全性*，並會於星期二的每個修補程式發行。

您可以使用任何已備妥的工具和流程來安裝更新。 唯一的差別在於系統必須使用上一節產生的金鑰來註冊，才能下載並安裝更新。

針對 Azure VM，系統會自動為您完成啟用電腦延伸安全性更新的程序。 應下載並安裝更新，而不需要額外設定。
