---
title: 使用管理工具的網路原則伺服器管理
description: 您可以使用本主題來了解您可用來管理 Windows Server 2016 中的網路原則伺服器的工具。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa1767e49b16f4a55f36e052d4354aaead540f26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856879"
---
# <a name="network-policy-server-management-with-administration-tools"></a>使用管理工具的網路原則伺服器管理

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來了解您可用來管理您 NPSs 的工具。

在安裝 NPS 之後，您可以管理 NPSs:

- 在本機，藉由使用 NPS Microsoft Management Console \(MMC\)嵌入式管理單元、 靜態的 NPS 主控台，在 系統管理工具、 Windows PowerShell 命令或網路骨架\(Netsh\)用於 NPS 的命令。
- 從遠端 NPS，使用 NPS MMC 嵌入式管理單元、 用於 NPS 的 Netsh 命令、 Windows PowerShell 命令為 NPS 中或遠端桌面連線。
- 從遠端工作站，搭配其他工具，例如 NPS MMC 或 Windows PowerShell 中使用遠端桌面連線。

>[!NOTE]
>在 Windows Server 2016 中，您可以使用 NPS 主控台來管理本機 NPS。 若要管理遠端和本機 NPSs，您必須使用 NPS MMC 嵌入式管理單元\-中。

下列各節會提供有關如何管理您的本機和遠端 NPSs 的指示。

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>設定本機 NPS 使用 NPS 主控台

您已安裝 NPS 之後，您可以使用此程序來管理本機 NPS 使用 NPS MMC。

**系統管理認證** 

若要完成此程序，您必須是 Administrators 群組的成員。

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>若要設定本機 NPS 使用 NPS 主控台

1. 在 [伺服器管理員] 中，按一下**工具**，然後按一下**網路原則伺服器**。 NPS 主控台隨即開啟。

2. 在 NPS 主控台中，按一下 NPS\(本機\)。 在 [詳細資料] 窗格中，選擇**標準組態**或**進階組態**，然後執行根據您選取下列其中一項：
    - 如果您選擇**標準組態**，從清單中，選取案例，然後依照 啟動組態精靈的指示。
    - 如果您選擇**進階組態**，按一下箭號以展開**進階組態選項**，，然後檢閱並設定可用的選項，您需要的 NPS 功能RADIUS 伺服器、 RADIUS proxy，或兩者。

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>使用 NPS MMC 嵌入式管理單元來管理多個 NPSs\-中

您可以使用此程序，使用 NPS MMC 嵌入式管理單元管理本機 NPS 和多個遠端 NPSs\-中。

然後再執行下列程序，您必須在本機電腦和遠端電腦上安裝 NPS。

根據網路狀況和 NPSs 數目您管理使用 NPS MMC 嵌入式管理單元\-MMC 嵌入式管理單元的回應中\-中可能會變慢。 此外，NPS 的組態資料傳輸透過網路傳送遠端系統管理工作階段期間使用 NPS 嵌入式管理單元\-中。 請確定您的網路的實體安全性和惡意的使用者沒有存取此網路流量。

**系統管理認證** 

若要完成此程序，您必須是 Administrators 群組的成員。

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>若要使用 NPS 嵌入式管理單元來管理多個 NPSs\-中

1. 若要開啟 MMC，請系統管理員身分執行 Windows PowerShell。 在 Windows PowerShell 中輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console 便會隨即開啟。
2. 在 MMC 中，在**檔案**功能表上，按一下**新增/移除嵌入式管理單元\-在**。 **新增或移除嵌入式管理單元\-ins**對話方塊隨即開啟。
3. 中**新增或移除嵌入式管理單元\-單元**，請在**可用的嵌入式管理單元\-集**，向下捲動清單，按一下 **網路原則伺服器**，然後按一下**新增**。 **選取電腦**對話方塊隨即開啟。
4. 在 **選取的電腦**，確認**本機電腦\(執行這個主控台的電腦\)** 已選取，然後按一下**確定**。 貼齊\-在本機 nps 會加入到清單中**選取嵌入式管理單元\-集**。
5. 中**新增或移除嵌入式管理單元\-ins**，請在**可用的嵌入式管理單元\-集**，確定**網路原則伺服器**保持選取，然後按一下  **新增**。 **選取電腦**對話方塊就會再次開啟。
6. 在 **選取電腦**，按一下**另一部電腦**，然後輸入 IP 位址或完整的網域名稱\(FQDN\)您想要使用 NPS 來管理遠端 NPS 的貼齊\-中。 或者，您可以按一下**瀏覽**閒時您想要新增之電腦的目錄。 按一下 [確定] 。
7. 重複步驟 5 和 6 NPS 嵌入式管理單元中加入更多的 NPSs\-中。 當您新增所有您想要管理，請按一下 NPSs**確定**。
8. 若要儲存以供稍後使用 NPS 嵌入式管理單元，請按一下**檔案**，然後按一下**儲存**。 在 [**另存新檔**] 對話方塊中，瀏覽至您要儲存檔案，硬碟位置輸入的名稱，您的 Microsoft Management console \(.msc\)檔案，然後再按一下**儲存**. 

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>使用遠端桌面連線來管理 NPS

您可以使用此程序來管理遠端 NPS 使用遠端桌面連線。

藉由使用遠端桌面連線，您可以從遠端管理執行 Windows Server 2016 程式 NPSs。 您可以也從遠端管理 NPSs 從執行 Windows 10 或更早的 Windows 用戶端作業系統的電腦。

您可以使用遠端桌面連線來管理多個 NPSs 使用兩種方法之一。

1. 個別建立每個您 NPSs 的遠端桌面連線。
2. 使用遠端桌面連接到單一 NPS，，然後在該伺服器上使用 NPS MMC 來管理其他遠端伺服器。 如需詳細資訊，請參閱上一節**使用 NPS MMC 嵌入式管理單元來管理多個 NPSs\-在**。

**系統管理認證** 

若要完成此程序，您必須是 NPS 上的 Administrators 群組的成員。

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>若要使用遠端桌面連線來管理 NPS

1. 在您想要管理遠端電腦上，在 [伺服器管理員] 中，每個 NPS 上選取**本機伺服器**。 在 [伺服器管理員的詳細資料] 窗格中，檢視**遠端桌面**設定，然後執行下列其中一項。 
    1. 如果值**遠端桌面**設定為**已啟用**，您不需要此程序中執行的一些步驟。 直接跳到步驟 4，以開始設定遠端桌面使用者的權限。
    2. 如果**遠端桌面**設定為**停用**，按一下 word**已停用**。 **系統屬性** 對話方塊會開啟**遠端** 索引標籤。
2. 在 **遠端桌面**，按一下**允許遠端連線到此電腦**。 **遠端桌面連線**對話方塊隨即開啟。 執行下列其中一項。
    1. 若要自訂允許的網路連線，請按一下**具有進階安全性的 Windows 防火牆**，然後設定您想要允許的設定。 
    2. 若要啟用遠端桌面連線，所有網路連線的電腦上，按一下**確定**。
3. 在 **系統屬性**，請在**遠端桌面**，決定是否要啟用**允許僅來自執行含有網路層級驗證遠端桌面的電腦連線**，然後選擇您的選項。
4. 按一下 **\[選取使用者\]**。 **Remote Desktop Users**對話方塊隨即開啟。
5. 在  **Remote Desktop Users**，若要從遠端連線至 NPS，請按一下 授與權限**新增**，然後輸入 使用者帳戶的使用者名稱。 按一下 [確定] 。
6. 您要遠端存取權限授與 NPS 每位使用者重複步驟 5。 當您完成新增使用者時，按一下**確定**關閉**Remote Desktop Users**  對話方塊並**確定**  以關閉 **系統屬性** 對話方塊。
7. 若要連線到遠端 NPS，您已使用先前的步驟，請按一下**開始**，依字母順序排列的清單向下捲動，然後按一下**Windows 附屬應用程式**，然後按一下**遠端桌面連線**。 **遠端桌面連線**對話方塊隨即開啟。
8. 在 **遠端桌面連線**對話方塊中，於**電腦**，輸入 NPS 名稱或 IP 位址。 如果想要的話，按一下**選項**，設定其他連接選項，然後按一下**儲存**儲存供重複使用連線。
9. 按一下  **Connect**，並出現提示時提供可登入，並將 NPS 設定的權限的帳戶的使用者帳戶認證。

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>使用 Netsh NPS 命令來管理 NPS

您可以使用 Netsh NPS 內容中的命令，以顯示和設定的驗證、 授權、 計量以及稽核由 NPS 與遠端存取服務的資料庫。 使用 Netsh NPS 內容中的命令：

- 設定或重新設定 NPS 中，包括所有層面也有設定使用 Windows 介面中的 NPS 主控台的 NPS。
- 匯出其中一個 NPS （來源伺服器），包括登錄機碼和 NPS 設定存放區，做為 Netsh 指令碼設定。
- 使用來源 NPS 的 Netsh 指令碼和匯出的組態檔，該設定匯入另一部 NPS。

從 Windows Server 2016 命令提示字元，或從 Windows PowerShell，您可以執行這些命令。 您也可以執行 nps 的 netsh 命令指令碼和批次檔中。

**系統管理認證** 

若要執行此程序，您必須是本機電腦上的 Administrators 群組成員。

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>輸入 Netsh NPS 內容上的 NPS

1. 開啟命令提示字元] 或 [Windows PowerShell。
2. 型別**netsh**，然後按 ENTER 鍵。
3. 型別**nps**，然後按 ENTER 鍵。
4. 若要檢視可用的命令清單，請輸入 問號\(？\)按 ENTER 鍵。


如需 Netsh NPS 命令的詳細資訊，請參閱[Windows Server 2008 中的網路原則伺服器的 Netsh 命令](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx)，或下載整個[Netsh 技術參考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0)從 TechNet 組件庫。 此下載是完整的網路介面技術參考適用於 Windows Server 2008 和 Windows Server 2008 R2。 格式是 Windows 協助\(*.chm\) zip 檔案中。 這些命令仍然會在 Windows Server 2016 和 Windows 10，因此雖然建議使用 Windows PowerShell，您可以使用 netsh，在這些環境中。

## <a name="use-windows-powershell-to-manage-npss"></a>使用 Windows PowerShell 來管理 NPSs

您可以使用 Windows PowerShell 命令來管理 NPSs。 如需詳細資訊，請參閱下列 Windows PowerShell 命令參考主題。

- [網路原則伺服器 (NPS) Cmdlet，在 Windows PowerShell 中的](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx)。 您可以使用這些 Windows Server 2012 R2 或更新版本的作業系統中的 netsh 命令。
- [NPS 模組](https://technet.microsoft.com/itpro/powershell/windows/nps/index)。 Windows Server 2016 中，您可以使用下列 netsh 命令。

如需 NPS 管理的詳細資訊，請參閱[管理網路原則伺服器 (NPS)](nps-manage-top.md)。
