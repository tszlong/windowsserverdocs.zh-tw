---
title: 將 Windows Server 2012 升級至 Windows Server 2016 |Microsoft Docs
description: 瞭解如何執行就地升級，以從 Windows Server 2012 至 Windows Server 2016。
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: 09c5a95e2ccd065f3ebbe551404064c39f803f2a
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124718"
---
# <a name="upgrade-windows-server-2012-to-windows-server-2016"></a>將 Windows Server 2012 升級至 Windows Server 2016

如果您想要保留相同的硬體和所有已設定的伺服器角色，而不簡維伺服器，您會想要進行就地升級。 就地升級可讓您從較舊的作業系統移至較新的作業系統，同時讓您的設定、伺服器角色和資料保持不變。 本文可協助您從 Windows Server 2012 移至 Windows Server 2016。

若要升級至 Windows Server 2019，請先使用本主題升級至 Windows Server 2016，然後[從 Windows server 2016 升級至 Windows server 2019](upgrade-2016-to-2019.md)。

## <a name="before-you-begin-your-in-place-upgrade"></a>開始進行就地升級之前

在您開始 Windows Server 升級之前，建議您從裝置收集一些資訊，以供診斷和疑難排解之用。 因為這項資訊僅適用于您的升級失敗的情況，所以您必須確定將資訊儲存在您可以從裝置中取得的任何地方。

### <a name="to-collect-your-info"></a>若要收集您的資訊

1. 開啟命令提示字元，移至`c:\Windows\system32`，然後輸入**systeminfo**。

2. 複製、貼上並將產生的系統資訊儲存在裝置的某個地方。

3. 在命令提示字元中輸入**ipconfig/all** ，然後將產生的設定資訊複製並貼到與上述相同的位置。

4. 開啟 [登錄編輯程式]，移至 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion hive，然後將 Windows Server **BuildLabEx** （版本）和**EditionID** （版本）複製並貼到與上述相同的位置。

收集所有 Windows Server 相關資訊之後，強烈建議您備份您的作業系統、應用程式和虛擬機器。 您也必須**關閉**、**快速遷移**或**即時移轉**目前正在伺服器上執行的任何虛擬機器。 就地升級期間，您不能有任何虛擬機器正在執行。

## <a name="to-perform-the-upgrade"></a>執行升級

1. 請確定**BuildLabEx**值指出您正在執行 Windows Server 2012。

2. 找出 [Windows Server 2016 安裝程式] 媒體，**然後選取 [setup.exe]** 。

    ![顯示 setup.exe 檔案的 Windows Explorer](media/upgrade-2012-2016/setup-2016.png)

3. 選取 **[是]** 以開始安裝程式。

    ![使用者帳戶控制，要求啟動安裝程式的許可權](media/upgrade-2012-2016/start-setup-uac-box.png)

4. 在 [Windows Server 2016] 畫面上，選取 [**立即安裝**]。

5. 針對連線到網際網路的裝置，選取 **[下載並安裝更新] （建議選項）** 。

    ![選擇上線以取得重要 Windows 更新的畫面](media/upgrade-2012-2016/imp-updates-win-setup.png)

6. 安裝程式會檢查您的裝置設定，您必須等候它完成，然後選取 **[下一步]** 。

7. 視您收到的 Windows Server 媒體（零售、大量授權、OEM、ODM 等）和伺服器授權而定，系統可能會提示您輸入授權金鑰以繼續。

    ![可供您輸入產品金鑰的畫面](media/upgrade-2012-2016/enter-product-key.png)

8. 選取您要安裝的 Windows Server 2016 版本，然後選取 **[下一步]** 。

    ![選擇要安裝之 Windows Server 2016 版本的畫面](media/upgrade-2012-2016/select-os-edition.png)

9. 選取 [**接受**] 以根據您的散發管道（例如，零售、大量授權、OEM、ODM 等等）接受授權合約的條款。

    ![接受授權合約的畫面](media/upgrade-2012-2016/license-terms.png)

10. 選取 [**保留個人檔案和應用程式**] 選擇進行就地升級，然後選取 **[下一步]** 。

    ![選擇安裝類型的畫面](media/upgrade-2012-2016/choose-install-upgrade.png)

11. 如果您看到通知您不建議升級的頁面，您可以忽略它，然後選取 [**確認**]。 它已放在提示進行全新安裝，但並不是必要的。

    ![顯示 [確認] 按鈕以確保您想要進行升級的畫面](media/upgrade-2012-2016/confirm-upgrade-process.png)

12. 安裝程式會告訴您使用 [**新增/移除程式**] 移除 Microsoft Endpoint Protection。

    這項功能與 Windows 10 不相容。

13. 安裝程式分析您的裝置之後，會提示您選取 [**安裝**] 來繼續升級。

    ![顯示您已準備好開始升級的畫面](media/upgrade-2012-2016/ready-to-install.png)

    就地升級會啟動，並顯示 [升級] **Windows**畫面及其進度。 升級完成之後，您的伺服器將會重新開機。

    ![顯示升級進度的畫面](media/upgrade-2012-2016/upgrading-windows-with-progress.png)

## <a name="after-your-upgrade-is-done"></a>完成升級之後

升級完成之後，您必須確定升級至 Windows Server 2016 已成功。

### <a name="to-make-sure-your-upgrade-was-successful"></a>確保升級成功

1. 開啟 [登錄編輯程式]，移至 [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion] hive，然後查看 [ **ProductName**]。 您應該會看到您的 Windows Server 2016 版本，例如**Windows server 2016 Datacenter**。

2. 請確定您所有的應用程式都在執行中，且您的用戶端連線成功。

如果您認為升級期間可能發生錯誤，請複製並壓縮`%SystemRoot%\Panther` （通常`C:\Windows\Panther`）目錄，並聯絡 Microsoft 支援服務。

## <a name="next-steps"></a>後續步驟

您可以再執行一次，從 Windows Server 2016 升級至 Windows Server 2019。 如需詳細指示，請參閱[將 Windows server 2016 升級至 Windows server 2019](upgrade-2016-to-2019.md)。

## <a name="related-articles"></a>相關文章

- 如需 Windows Server 2016 的詳細資訊和詳細資訊，請參閱[開始使用 Windows server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics)。