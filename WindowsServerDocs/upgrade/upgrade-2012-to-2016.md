---
title: 將 Windows Server 2012 升級至 Windows Server 2016 | Microsoft Docs
description: 了解如何執行就地升級，從 Windows Server 2012 升級至 Windows Server 2016。
ms.topic: how-to
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 895af82c3afb25a064aab8e2878b405c8a6a38c5
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949634"
---
# <a name="upgrade-windows-server-2012-to-windows-server-2016"></a>將 Windows Server 2012 升級至 Windows Server 2016

如果您想要在不使伺服器平階化的情況下保留相同的硬體和您已設定的所有伺服器角色，您可以執行就地升級。 就地升級可讓您從舊版作業系統升級至新版本，且您的設定、伺服器角色和資料將維持不變。 本文可協助您從 Windows Server 2012 移至 Windows Server 2016。

若要升級至 Windows Server 2019，請先使用本主題升級至 Windows Server 2016，然後再[從 Windows Server 2016 升級至 Windows Server 2019](upgrade-2016-to-2019.md)。

## <a name="before-you-begin-your-in-place-upgrade"></a>開始進行就地升級之前

開始進行 Windows Server 升級之前，建議您從裝置收集一些資訊，以供診斷和疑難排解之用。 由於這些資訊預期只會在您的升級失敗時使用，因此請務必將資訊儲存在某個您可從裝置取得之處。

### <a name="to-collect-your-info"></a>收集您的資訊

1. 開啟命令提示字元，移至 `c:\Windows\system32`，然後輸入 **systeminfo.exe**。

2. 複製並貼上產生的系統資訊，然後儲存至裝置以外的某處。

3. 在命令提示字元中輸入 **ipconfig /all**，然後將產生的組態資訊複製並貼到前述的相同位置。

4. 開啟登錄編輯程式，移至 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion hive，然後將 Windows Server **BuildLabEx** (版本) 和 **EditionID** (版次) 複製並貼到前述的相同位置。

收集所有 Windows Server 相關資訊之後，強烈建議您備份作業系統、應用程式和虛擬機器。 您也必須 **關閉**、**快速移轉** 或 **即時移轉** 目前在伺服器上執行的任何虛擬機器。 在就地升級期間，不可以有任何執行中的虛擬機器。

## <a name="to-perform-the-upgrade"></a>執行升級

1. 確定 **BuildLabEx** 值指出您正在執行 Windows Server 2012。

2. 找出 Windows Server 2016 安裝程式媒體，然後選取 **setup.exe**。

    ![顯示 setup.exe 檔案的 Windows 檔案總管](media/upgrade-2012-2016/setup-2016.png)

3. 選取 [是]  以啟動安裝程序。

    ![使用者帳戶控制要求啟動安裝程序的權限](media/upgrade-2012-2016/start-setup-uac-box.png)

4. 在 [Windows Server 2016] 畫面上，選取 [立即安裝]  。

5. 針對連線至網際網路的裝置，選取 [下載並安裝更新 (建議選項)]  。

    ![選擇連線以取得重要 Windows 更新的畫面](media/upgrade-2012-2016/imp-updates-win-setup.png)

6. 安裝程式會檢查您的裝置組態，您必須等候該檢查完成，然後選取 [下一步]  。

7. 根據您據以取得 Windows Server 媒體 (零售、大量授權、OEM、ODM 等) 的散發通路和伺服器授權，系統可能會提示您輸入授權金鑰以繼續操作。

    ![可供您輸入產品金鑰的畫面](media/upgrade-2012-2016/enter-product-key.png)

8. 選取您要安裝的 Windows Server 2016 版本，然後選取 [下一步]  。

    ![選擇要安裝哪個 Windows Server 2016 版本的畫面](media/upgrade-2012-2016/select-os-edition.png)

9. 根據您的散發通路 (例如，零售、大量授權、OEM、ODM 等)，選取 [接受]  以接受授權合約的條款。

    ![接受授權合約的畫面](media/upgrade-2012-2016/license-terms.png)

10. 選取 [保留個人檔案與應用程式]  以選擇執行就地升級，然後選取 [下一步]  。

    ![選擇安裝類型的畫面](media/upgrade-2012-2016/choose-install-upgrade.png)

11. 如果您看到建議您不要升級的頁面，您可加以忽略，然後選取 [確認]  。 該提示是針對全新安裝而設置的，但並非必要。

    ![此畫面顯示表示確定要進行升級的 [確認] 按鈕](media/upgrade-2012-2016/confirm-upgrade-process.png)

12. 安裝程式會指示您使用 [新增/移除程式]  將 Microsoft Endpoint Protection 移除。

    此功能與 Windows 10 不相容。

13. 安裝程式在分析您的裝置之後，會提示您選取 [安裝]  以繼續進行升級。

    ![顯示您已準備好可開始升級的畫面](media/upgrade-2012-2016/ready-to-install.png)

    就地升級隨即啟動，並顯示 [正在升級 Windows]  畫面及其進度。 升級完成後，您的伺服器將會重新啟動。

    ![顯示升級進度的畫面](media/upgrade-2012-2016/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>完成升級之後

升級完成之後，您必須確定升級至 Windows Server 2016 的作業已成功。

### <a name="to-make-sure-your-upgrade-was-successful"></a>確定升級成功

1. 開啟登錄編輯程式，並移至 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion hive，然後檢視 **ProductName**。 您應該會看到您的 Windows Server 2016 版本，例如 **Windows Server 2016 Datacenter**。

2. 確定您所有的應用程式都在執行中，且用戶端已成功連線至應用程式。

如果您認為在升級期間可能發生錯誤，請複製並壓縮 `%SystemRoot%\Panther` (通常是 `C:\Windows\Panther`) 目錄，並連絡 Microsoft 支援服務。

## <a name="next-steps"></a>接下來的步驟

您可以進一步執行從 Windows Server 2016 到 Windows Server 2019 的就地升級。 如需詳細指示，請參閱[將 Windows Server 2016 升級至 Windows Server 2019](upgrade-2016-to-2019.md)。

## <a name="related-articles"></a>相關文章

- 如需關於 Windows Server 2016 的詳細資訊，請參閱[開始使用 Windows Server 2016](../get-started/server-basics.md)。