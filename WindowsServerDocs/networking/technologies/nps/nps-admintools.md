---
title: 使用管理工具的網路原則伺服器管理
description: 您可以使用本主題來瞭解可用於管理 Windows Server 2016 中網路原則伺服器的工具。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1bb6447197bfed1108a62be077b0a076bef995da
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316352"
---
# <a name="network-policy-server-management-with-administration-tools"></a>使用管理工具的網路原則伺服器管理

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解您可以用來管理 Nps 的工具。

安裝 NPS 之後，您可以管理 Nps：

- 在本機上，藉由使用 NPS Microsoft Management Console \(MMC\) 嵌入式管理單元、[系統管理工具]、[Windows PowerShell 命令] 或 [網路介面] 中的靜態 NPS 主控台 \(適用于 NPS 的 Netsh\) 命令。
- 從遠端 NPS，使用 NPS MMC 嵌入式管理單元、NPS 的 Netsh 命令、NPS 的 Windows PowerShell 命令，或遠端桌面連線。
- 從遠端工作站，使用遠端桌面連線與其他工具（例如 NPS MMC 或 Windows PowerShell）結合。

>[!NOTE]
>在 Windows Server 2016 中，您可以使用 NPS 主控台來管理本機 NPS。 若要同時管理遠端和本機 Nps，您必須使用中的 NPS MMC 嵌入式管理\-。

下列各節提供有關如何管理本機和遠端 Nps 的指示。

## <a name="configure-the-local-nps-by-using-the-nps-console"></a>使用 NPS 主控台設定本機 NPS

安裝 NPS 之後，您可以使用此程式，使用 NPS MMC 來管理本機 NPS。

**系統管理認證** 

若要完成此程序，您必須是 Administrators 群組的成員。

### <a name="to-configure-the-local-nps-by-using-the-nps-console"></a>使用 NPS 主控台設定本機 NPS

1. 在伺服器管理員中，按一下 [**工具**]，然後按一下 [**網路原則伺服器**]。 NPS 主控台隨即開啟。

2. 在 NPS 主控台中，按一下 [NPS \(本機\)]。 在詳細資料窗格中，選擇 [**標準**設定] 或 [ **Advanced configuration**]，然後根據您的選擇執行下列其中一項：
    - 如果您選擇 [**標準**設定]，請從清單中選取案例，然後依照指示來啟動設定向導。
    - 如果您選擇 [ **Advanced configuration**]，請按一下箭號以展開 [ **advanced configuration options**]，然後根據您想要的 NPS 功能（radius 伺服器、radius proxy 或兩者）來檢查並設定可用的選項。

## <a name="manage-multiple-npss-by-using-the-nps-mmc-snap-in"></a>使用中的 NPS MMC 嵌入式管理\-管理多個 Nps

您可以使用這個程式，利用中的 NPS MMC 嵌入式管理單元\-來管理本機 NPS 和多個遠端 Nps。

執行下列程式之前，您必須在本機電腦和遠端電腦上安裝 NPS。

根據網路狀況，以及您使用 NPS MMC 嵌入式管理單元管理\-的 Nps 數目而定，中 MMC 嵌入式管理單元\-的回應可能會變慢。 此外，NPS 設定流量會在遠端系統管理會話期間透過網路傳送，方法是使用中的 NPS 嵌入式管理單元\-。 請確定您的網路實體安全，而且惡意使用者沒有此網路流量的存取權。

**系統管理認證** 

若要完成此程序，您必須是 Administrators 群組的成員。

### <a name="to-manage-multiple-npss-by-using-the-nps-snap-in"></a>若要使用中的 NPS 嵌入式管理單元\-管理多個 Nps

1. 若要開啟 MMC，請以系統管理員身分執行 Windows PowerShell。 在 Windows PowerShell 中，輸入**mmc**，然後按 enter。 Microsoft Management Console 便會隨即開啟。
2. 在**MMC 的 [檔案] 功能表上**，按一下 [**新增/移除嵌入式管理單元\-** ]。 [**新增或移除嵌入式管理單元\-** 的] 對話方塊隨即開啟。
3. 在 [**新增或移除嵌入式管理單元\-** ] 的 [可用的嵌入式**管理單元\-** ] 中，依序向下清單、[**網路原則伺服器**]，然後按一下 [**新增**]。 [**選取電腦**] 對話方塊隨即開啟。
4. 在 [**選取電腦**] 中，確認已選取 **[本機電腦 \(執行此主控台的電腦\)** ]，然後按一下 **[確定]** 。 本機 NPS 的貼齊\-會新增至**所選嵌入式管理單元\-** 中的清單。
5. 在 [**新增或移除嵌入式管理單元\-** ] 的 [可用的嵌入式**管理單元\-** ] 中，確定 [**網路原則伺服器**] 仍為選取狀態，然後按一下 [**新增**]。 [**選取電腦**] 對話方塊隨即開啟。
6. 在 [**選取電腦**] 中，按一下 [**另一台電腦**]，然後輸入您想要使用中的 NPS 嵌入式管理\-管理之遠端 NPS 的 IP 位址或完整功能變數名稱 \(FQDN\)。 （選擇性）您可以按一下 **[流覽]** ，詳閱您想要新增之電腦的目錄。 按一下 [確定]。
7. 重複步驟5和6，將更多 Nps 新增至中的 NPS 嵌入式管理單元\-。 當您已新增所有想要管理的 Nps 時，請按一下 **[確定]** 。
8. 若要儲存 NPS 嵌入式管理單元以供稍後**使用，請按一下 [** 檔案]，然後按一下 [**儲存**]。 在 [**另存**新檔] 對話方塊中，流覽至您要儲存檔案的硬碟位置，並在 [您的 Microsoft Management Console \(]\) 檔案中輸入名稱，然後按一下 [**儲存**]。 

## <a name="manage-an-nps-by-using-remote-desktop-connection"></a>使用遠端桌面連線管理 NPS

您可以使用這個程式，利用遠端桌面連線來管理遠端 NPS。

藉由使用遠端桌面連線，您可以從遠端系統管理執行 Windows Server 2016 的 Nps。 您也可以從執行 Windows 10 或舊版 Windows 用戶端作業系統的電腦遠端系統管理 Nps。

您可以使用 [遠端桌面連線]，透過兩種方法的其中一種來管理多個 Nps。

1. 為每個 Nps 個別建立遠端桌面連線。
2. 使用 [遠端桌面] 連線到一個 NPS，然後在該伺服器上使用 NPS MMC 來管理其他遠端伺服器。 如需詳細資訊，請參閱上一節**使用 NPS mmc 嵌入式管理單元\-管理多個 nps**。

**系統管理認證** 

若要完成此程式，您必須是 NPS 上 Administrators 群組的成員。

### <a name="to-manage-an-nps-by-using-remote-desktop-connection"></a>若要使用遠端桌面連線來管理 NPS

1. 在您要遠端系統管理的每個 NPS 上，選取 伺服器管理員中的 **本機伺服器**。 在 [伺服器管理員詳細資料] 窗格中，查看 [**遠端桌面**] 設定，然後執行下列其中一項動作。 
    1. 如果 [**遠端桌面**] 設定的值為 [**已啟用**]，則不需要執行此程式中的部分步驟。 向下跳至步驟4，開始設定遠端桌面使用者權限。
    2. 如果 [**遠端桌面**] 設定已**停用**，請按一下 [**停用**] 這個字。 [**系統屬性**] 對話方塊會在 [**遠端**] 索引標籤上開啟。
2. 在 [**遠端桌面**] 中，按一下 [**允許此電腦的遠端連線**]。 [**遠端桌面連線**] 對話方塊隨即開啟。 執行下列其中一個步驟。
    1. 若要自訂允許的網路連線，請按一下 [**具有 Advanced Security 的 Windows 防火牆**]，然後設定您想要允許的設定。 
    2. 若要啟用電腦上所有網路連線的遠端桌面連線，請按一下 **[確定]** 。
3. 在 [**系統**內容] 的 [**遠端桌面**] 中，決定是否要啟用 [**僅允許來自執行具有網路層級驗證之遠端桌面的電腦**進行連線]，然後進行選取。
4. 按一下 **\[選取使用者\]** 。 [**遠端桌面使用者**] 對話方塊隨即開啟。
5. 在 [**遠端桌面使用者**] 中，若要授與使用者遠端連線至 NPS 的許可權，請按一下 [**新增**]，然後輸入使用者帳戶的使用者名稱。 按一下 [確定]。
6. 針對您要授與 NPS 遠端存取許可權的每個使用者，重複步驟5。 當您完成新增使用者時，按一下 **[確定**] 關閉 [**遠端桌面使用者**] 對話方塊，然後再按一次 **[確定]** 關閉 [**系統**內容] 對話方塊。
7. 若要連線到您使用先前步驟所設定的遠端 NPS，請按一下 [**開始**]，依字母順序向下清單，再按一下 [ **Windows 附屬**應用程式]，然後按一下 [**遠端桌面連線**]。 [**遠端桌面連線**] 對話方塊隨即開啟。
8. 在 [**遠端桌面連線**] 對話方塊的 [**電腦**] 中，輸入 NPS 名稱或 IP 位址。 如果您想要的話，請按一下 [**選項**]、[設定其他連接選項]，然後按一下 [**儲存**] 以儲存連接以供重複使用。
9. 按一下 **[連線]** ，然後在出現提示時，提供具有登入和設定 NPS 許可權之帳戶的使用者帳號憑證。

## <a name="use-netsh-nps-commands-to-manage-an-nps"></a>使用 Netsh NPS 命令管理 NPS

您可以使用 Netsh NPS 內容中的命令，來顯示和設定 NPS 和「遠端存取」服務所使用的驗證、授權、帳戶處理和審核資料庫的設定。 使用 Netsh NPS 內容中的命令來執行下列動作：

- 設定或重新設定 NPS，包括也可以在 Windows 介面中使用 NPS 主控台進行設定的所有 NPS 層面。
- 將一個 NPS （來源伺服器）的設定（包括登錄機碼和 NPS 設定存放區）匯出為 Netsh 腳本。
- 使用 Netsh 腳本和從來源 NPS 匯出的設定檔，將設定匯入另一個 NPS。

您可以從 Windows Server 2016 命令提示字元或從 Windows PowerShell 執行這些命令。 您也可以在腳本和批次檔中執行 netsh nps 命令。

**系統管理認證** 

若要執行此程序，您必須是本機電腦上的 Administrators 群組成員。

### <a name="to-enter-the-netsh-nps-context-on-an-nps"></a>在 NPS 上輸入 Netsh NPS 內容

1. 開啟 [命令提示字元] 或 [Windows PowerShell]。
2. 輸入**netsh**，然後按 enter。
3. 輸入**nps**，然後按 enter。
4. 若要查看可用的命令清單，請在 \(中輸入問號？\)，然後按 ENTER 鍵。


如需 Netsh NPS 命令的詳細資訊，請參閱[Windows Server 2008 中網路原則伺服器的 Netsh 命令](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx)，或從 TechNet 資源庫下載完整的[netsh 技術參考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0)。 此下載是 Windows Server 2008 和 Windows Server 2008 R2 的完整網路 Shell 技術參考。 格式為 zip 檔案中的 Windows Help \(* .chm\)。 這些命令仍然存在於 Windows Server 2016 和 Windows 10 中，因此您可以在這些環境中使用 netsh，但建議使用 Windows PowerShell。

## <a name="use-windows-powershell-to-manage-npss"></a>使用 Windows PowerShell 管理 Nps

您可以使用 Windows PowerShell 命令來管理 Nps。 如需詳細資訊，請參閱下列 Windows PowerShell 命令參考主題。

- [Windows PowerShell 中的網路原則伺服器（NPS） Cmdlet](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx)。 您可以在 Windows Server 2012 R2 或更新版本的作業系統中使用這些 netsh 命令。
- [NPS 模組](https://technet.microsoft.com/itpro/powershell/windows/nps/index)。 您可以在 Windows Server 2016 中使用這些 netsh 命令。

如需 NPS 系統管理的詳細資訊，請參閱[管理網路原則伺服器（NPS）](nps-manage-top.md)。
