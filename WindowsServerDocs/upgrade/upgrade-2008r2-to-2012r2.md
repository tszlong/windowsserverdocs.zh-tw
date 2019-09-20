---
title: 將 Windows Server 2008 R2 升級至 Windows Server 2012 R2 |Microsoft Docs
description: 瞭解如何執行就地升級，以從 Windows Server 2008 R2 移至 Windows Server 2012 R2。
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/16/2019
ms.openlocfilehash: d5051239f7269eb4b6361187121ac960e06f6d9e
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2019
ms.locfileid: "71125078"
---
# <a name="upgrade-windows-server-2008-r2-to-windows-server-2012-r2"></a>將 Windows Server 2008 R2 升級至 Windows Server 2012 R2

如果您想要保留相同的硬體和所有已設定的伺服器角色，而不簡維伺服器，您會想要進行就地升級。 就地升級可讓您從較舊的作業系統移至較新的作業系統，同時讓您的設定、伺服器角色和資料保持不變。 本文可協助您從 Windows Server 2008 R2 移至 Windows Server 2012 R2。

若要升級至 Windows Server 2019，請先使用本主題升級至 Windows Server 2012 R2，然後再[從 Windows server 2012 R2 升級至 Windows server 2019](upgrade-2012r2-to-2019.md)。

## <a name="before-you-begin-your-in-place-upgrade"></a>開始進行就地升級之前

在您開始 Windows Server 升級之前，建議您從裝置收集一些資訊，以供診斷和疑難排解之用。 因為這項資訊僅適用于您的升級失敗的情況，所以您必須確定將資訊儲存在您可以從裝置中取得的任何地方。

### <a name="to-collect-your-info"></a>若要收集您的資訊

1. 開啟命令提示字元，移至`c:\Windows\system32`，然後輸入**systeminfo**。

2. 複製、貼上並將產生的系統資訊儲存在裝置的某個地方。

3. 在命令提示字元中輸入**ipconfig/all** ，然後將產生的設定資訊複製並貼到與上述相同的位置。

4. 開啟 [登錄編輯程式]，移至 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion hive，然後將 Windows Server **BuildLabEx** （版本）和**EditionID** （版本）複製並貼到與上述相同的位置。

收集所有 Windows Server 相關資訊之後，強烈建議您備份您的作業系統、應用程式和虛擬機器。 您也必須**關閉**、**快速遷移**或**即時移轉**目前正在伺服器上執行的任何虛擬機器。 就地升級期間，您不能有任何虛擬機器正在執行。

## <a name="to-perform-the-upgrade"></a>執行升級

1. 請確定**BuildLabEx**值指出您正在執行 Windows Server 2008 R2。

2. 找出 Windows Server 2012 R2 安裝媒體，**然後選取 [setup.exe]** 。

    ![顯示 setup.exe 檔案的 Windows Explorer](media/upgrade-2008r2-2012r2/setup-2012r2.png)

3. 選取 **[是]** 以開始安裝程式。

    ![使用者帳戶控制，要求啟動安裝程式的許可權](media/upgrade-2008r2-2012r2/start-setup-uac-box.png)

4. 在 [Windows Server 2012 R2] 畫面上，選取 [**立即安裝**]。

5. 若是已連線到網際網路的裝置，請選取 **[上線] 立即安裝更新（建議選項）** 。

    ![選擇上線以取得重要 Windows 更新的畫面](media/upgrade-2008r2-2012r2/imp-updates-win-setup.png)

6. 選取您要安裝的 Windows Server 2012 R2 版本，然後選取 **[下一步]** 。

    ![選擇要安裝之 Windows Server 2012 R2 版本的畫面](media/upgrade-2008r2-2012r2/select-os-edition.png)

7. 選取 **[我接受授權條款**]，根據您的散發管道（例如，零售、大量授權、OEM、ODM 等等）接受授權合約的條款，然後選取 **[下一步]** 。

    ![接受授權合約的畫面](media/upgrade-2008r2-2012r2/license-terms.png)

8. 選取**升級：安裝 Windows，並讓檔案、設定和應用**程式選擇進行就地升級。

    ![選擇安裝類型的畫面](media/upgrade-2008r2-2012r2/choose-install-upgrade.png)

9. 安裝程式會提醒您，使用[Windows server 安裝和升級](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade)一文中的資訊，確保您的應用程式與 windows Server 2012 R2 相容，然後選取 **[下一步]** 。

    ![螢幕會提醒您檢查應用程式相容性](media/upgrade-2008r2-2012r2/compatibility-report.png)

10. 如果您看到通知您不建議升級的頁面，您可以忽略它，然後選取 [**確認**]。 它已放在提示進行全新安裝，但並不是必要的。

    ![顯示升級進度的畫面](media/upgrade-2008r2-2012r2/upgrading-windows-with-progress.png)

    就地升級會啟動，並顯示 [升級] **Windows**畫面及其進度。 升級完成之後，您的伺服器將會重新開機。

## <a name="after-your-upgrade-is-done"></a>完成升級之後

升級完成之後，您必須確定升級至 Windows Server 2012 R2 成功。

### <a name="to-make-sure-your-upgrade-was-successful"></a>確保升級成功

1. 開啟 [登錄編輯程式]，移至 [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion] hive，然後查看 [ **ProductName**]。 您應該會看到您的 Windows Server 2012 R2 版本，例如**Windows server 2012 R2 Datacenter**。

2. 請確定您所有的應用程式都在執行中，且您的用戶端連線成功。

如果您認為升級期間可能發生錯誤，請複製並壓縮`%SystemRoot%\Panther` （通常`C:\Windows\Panther`）目錄，並聯絡 Microsoft 支援服務。

## <a name="next-steps"></a>後續步驟

您可以再執行一次，從 Windows Server 2012 R2 升級至 Windows Server 2019。 如需詳細指示，請參閱[將 Windows server 2012 R2 升級至 Windows server 2019](upgrade-2012r2-to-2019.md)。

## <a name="related-articles"></a>相關文章

- 如需有關 Windows Server 2012 R2 的詳細資訊和詳細資訊，請參閱[Windows server 2012 R2 和 Windows server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh801901(v=ws.11))。
