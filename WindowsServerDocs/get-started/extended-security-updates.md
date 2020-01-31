---
title: Windows Server 2008 和 2008 R2 延伸安全性更新
description: 瞭解如何在其支援週期結束後，使用 Windows Server 2008 和 2008 R2 延伸安全性更新 (ESU)。
ms.prod: windows-server
ms.technology: server-general
ms.mktglfcycl: manage
author: iainfoulds
ms.author: iainfou
ms.topic: get-started-article
ms.localizationpriority: high
ms.date: 01/23/2020
ms.openlocfilehash: 0f3ea0dacc200adaaec5064d19754ad6de0042a6
ms.sourcegitcommit: ff0db5ca093a31034ccc5e9156f5e9b45b69bae5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725773"
---
# <a name="how-to-use-windows-server-2008-and-2008-r2-extended-security-updates-esu"></a>如何使用 Windows Server 2008 和 2008 R2 延伸安全性更新 (ESU)

>適用於：Windows Server 2008 / 2008 R2

Windows Server 2008 和 Windows Server 2008 R2 支援週期將於 2020 年 1 月 14 日到期。 Windows Server 長期維護通道 (LTSC) 至少有十年的支援、五年的主要支援，以及五年的延伸支援。 這種支援包括定期的安全性更新。

結束支援也表示安全性更新的結束。 此案例可能會導致安全性或相容性問題，並讓商務應用程式面臨風險。 Microsoft 建議您[升級到目前版本的 Windows Server](modernize-windows-server-2008.md)，以獲得最先進的安全性、效能和創新。

如果您無法在支援生命週期期限結束前升級所有伺服器，下列選項會在升級轉換期間協助保護應用程式和資料：

* 將現有的 Windows Server 2008 和 2008 R2 工作負載依目前的方式遷移至 Azure 虛擬機器 (VM)。
    * 遷移到 Azure 會自動提供額外三年延伸安全性更新(ESU)。 延伸安全性更新在 Azure VM 成本上不會額外收費，而且不需要額外的設定。
* 請為您的伺服器購買延伸安全性更新訂閱，並保持受保護狀態，直到您準備好升級至更新的 Windows Server 版本。
    * 在支援生命週期日期結束之後，最多可提供三年的更新。

在三年期的延伸更新之後，電腦沒有任何選項可接收其他更新。

## <a name="what-are-extended-security-updates-for-windows-server"></a>什麼是 Windows Server 的延伸安全性更新？

適用於 Windows Server 的延伸安全性更新 (ESU) 包含安全性更新和公告，等級為*非常重要*和*重要*，自 2020 年 1月 14 日起最多三年。 延伸安全性更新不包含下列內容：

* 新功能
* 客戶要求的非安全性 Hotfix
* 設計變更要求

如需詳細資訊，請參閱[延伸安全性更新常見問題集](https://www.microsoft.com/cloud-platform/extended-security-updates)。

## <a name="how-to-use-extended-security-updates"></a>使用延伸安全性更新的方法

如果您在 Azure 中執行 Windows Server 2008/2008 R2 VM，其會自動啟用延伸安全性更新。 您不需要執行任何設定，也不需額外付費即可使用 Azure VM 的延伸安全性更新。 如果設定為接收更新，則延伸安全性更新會自動傳遞至 Azure VM。

針對其他環境 (例如內部部署 VM 或實體伺服器)，您必須手動要求並設定延伸安全性更新。 如果您已購買延伸安全性更新，這些更新可透過大量授權方案取得，例如 Enterprise 合約 (EA)、Enterprise 合約訂用帳戶 (EAS)、註冊教育解決方案 (EES)，或伺服器和雲端註冊 (SCE)，您可以使用下列其中一個步驟來取得啟用金鑰：

* 登入 [Microsoft 大量授權服務中心](https://www.microsoft.com/Licensing/servicecenter/default.aspx)以檢視並取得啟用金鑰。
* 在 Azure 入口網站中註冊獲得延伸安全性更新，以取得 Windows Server 2008/R2 啟用金鑰。
    * 如需完成此程序的步驟方法，請參閱本文中的下列步驟。

## <a name="register-for-extended-security-updates"></a>註冊延伸安全性更新

若要使用延伸安全性更新，您可以建立多個啟用金鑰(MAK)，並將其套用至 Windows Server 2008 和 2008 R2 電腦。 此金鑰可讓 Windows Update 伺服器知道您可以繼續接收安全性更新。 即使您只使用內部部署電腦，也能使用 Azure 入口網站來註冊延伸安全性更新及管理這些金鑰。

> [!NOTE]
>
> 如果您正在 Azure VM 上執行 Windows Server 2008 和 2008 R2，則不需要註冊延伸安全性更新。 對於其他環境 (例如內部部署 VM 或實體伺服器)，請先[購買延伸安全性更新](https://www.microsoft.com/licensing/how-to-buy/how-to-buy)，再嘗試註冊並使用該更新。

> [!IMPORTANT]
>
> 請確定您已遵循先前的步驟，透過大量授權方案購買延伸安全性更新。 依照下列步驟進行之前，請先傳送電子郵件至 [winsvresuchamps@microsoft.com](mailto:winsvresuchamps@microsoft.com)，並提供下列資訊以供核准使用該功能：
>
> * 客戶名稱：
> * Azure 訂用帳戶：
> * EA 合約編號 (適用於 ESU)：
> * ESU 伺服器數目：
>
> 小組會查看提供的資訊，並將使用者/訂用帳戶新增至核准清單。
>
> 如果要求者未列入核准清單，可能會發生下列錯誤：
>
> [在命名空間 'Microsoft.WindowsESU' 中找不到資源類型](https://social.msdn.microsoft.com/Forums/office/94b16a89-3149-43da-865d-abf7dba7b977/the-resource-type-could-not-be-found-in-the-namespace-microsoftwindowsesu-for-api-version)

若要為延伸安全性更新註冊非 Azure VM 並建立金鑰，請在 Azure 入口網站中完成下列步驟：

1. 登入 [Azure 入口網站](https://portal.azure.com/)。
1. 在 Azure 入口網站頂端的 [搜尋] 方塊中，搜尋並選取 [延伸安全性更新]  。

    ![搜尋 Azure 入口網站中的延伸安全性更新](media/extended-security-updates/esu-portal-search.png)

    如果您之前尚未使用過延伸安全性更新，請先選擇 [+ 建立]  延伸安全性更新資源。 否則，請從清單中選取您的資源。

1. 在 [註冊延伸服務更新]  底下，選取 [開始使用]  。

    ![Azure 入口網站中的延伸安全性更新入門](media/extended-security-updates/get-started-with-esu.png)

1. 若要建立您的第一個金鑰，請選取 [取得金鑰]  。

    ![選擇在 Azure 入口網站中建立金鑰](media/extended-security-updates/get-key.png)

    > [!NOTE]
    > 您需要與帳戶相關聯的 Azure 訂用帳戶，才能建立延伸安全性更新資源和金鑰。 如果您沒有與帳戶相關聯的 Azure 訂用帳戶，請以不同的使用者帳戶登入，或使用入口網站中所顯示的引導式步驟來建立 Azure 訂用帳戶。

1. 在 [Azure 詳細資料]  底下，選取您的 Azure 訂用帳戶、資源群組和金鑰的位置。

    在 [註冊詳細資料]  底下，輸入下列資訊：

    | 設定             | 值 |
    |---------------------|-------|
    | 索引鍵名稱            | 您金鑰的顯示名稱，例如 *Agreement01*。 |
    | 合約編號    | 大量授權合約管理系統所產生的合約編號，或 Enterprise 合約程式的 MSLicense。 |
    | 電腦數目 | 選擇您想要使用此金鑰安裝延伸安全性更新的電腦數目。 |
    | 作業系統    | 選擇要搭配使用此金鑰的作業系統，例如 Windows Server 2008 或 Windows Server 2008 R2。 |

    準備好時，請選取 [檢閱 + 註冊]  。

1. 驗證成功之後，就會顯示新登錄資源的選擇摘要。 如有需要，請更正任何驗證錯誤，或更新您的設定選擇。 Azure [使用規定](https://azure.microsoft.com/support/legal/)和[隱私權原則](https://privacy.microsoft.com/privacystatement)皆可使用。

    核取此方塊，以確認您有合格的電腦，且金鑰僅會在您的組織內使用：

    ![確認金鑰僅會由您的組織使用](media/extended-security-updates/confirm-key-usage.png)

    準備好時，請選取 [建立]  來產生 MAK。

延伸安全性更新註冊現在可與您的電腦搭配使用。 建立的金鑰應該套用到您想要保持符合安全性更新的 Windows Server 2008 和 2008 R2 電腦。

## <a name="download-and-apply-extended-security-updates"></a>下載並套用延伸安全性更新

Windows Server 延伸安全性更新的傳遞、下載及應用程式，與現有的部署程序並無不同。 透過延伸安全性更新提供的更新，僅適用於*安全性*，並會於星期二的每個修補程式發行。

您可以使用任何已備妥的工具和流程來安裝更新。 唯一的差別在於系統必須使用上一節產生的金鑰來註冊，才能下載並安裝更新。

針對 Azure VM，系統會自動為您完成啟用電腦延伸安全性更新的程序。 應下載並安裝更新，而不需要額外設定。