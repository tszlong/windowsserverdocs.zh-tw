---
title: 使用管理工具的網路原則伺服器管理
description: 若要深入了解可用來管理 Windows Server 2016 中的網路原則伺服器的工具，您可以使用此主題。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 5de80dc0-53be-42b7-8e5b-24d213bf2b25
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cdb82c5bc1c541f19b1b2f4c8db837af4a812f81
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-management-with-administration-tools"></a>使用管理工具的網路原則伺服器管理

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要深入了解您可以使用管理 NPS 伺服器工具，您可以使用此主題。

NPS 安裝之後，您可以管理 NPS 伺服器：

- 本機，來使用 NPS NPS Microsoft Management Console \(MMC\) 嵌入式管理單元，靜態 NPS 主控台系統管理工具，Windows PowerShell 命令，或網路殼層 \(Netsh\) 命令。
- 從遠端 NPS 伺服器、NPS，或遠端桌面連接使用 NPS MMC 嵌入式管理單元、NPS Netsh 命令、的 Windows PowerShell 命令。
- 從遠端工作站，藉由組合與其他工具，例如 NPS MMC 或 Windows PowerShell 中使用遠端桌面連接。

>[!NOTE]
>在 Windows Server 2016，您可以使用 NPS 主控台管理 NPS 本機伺服器。 若要管理遠端和本機 NPS 伺服器，您必須使用 NPS MMC snap\ 中。

下列章節提供如何管理您的本機與遠端 NPS 伺服器上的指示。

## <a name="configure-the-local-nps-server-by-using-the-nps-console"></a>使用 NPS 主機設定 NPS 本機伺服器

您已安裝 NPS 之後，您可以使用此程序使用 NPS MMC 管理 NPS 本機伺服器。

**管理認證** 

若要完成此程序，您必須是系統管理員群組成員。

### <a name="to-configure-the-local-nps-server-by-using-the-nps-console"></a>藉由使用 NPS 主機設定 NPS 本機伺服器

1. 在伺服器管理員中，按一下 [**工具**，然後按**的網路原則伺服器**。 NPS 主控台開啟。

2. 在 [NPS 主控台中，按一下 [NPS \(Local\)。 在詳細資料窗格中，選擇**標準設定**或**進階設定**，然後執行下列其中一個動作，根據您的選取項目：
    - 如果您選擇 [**標準設定**，從清單中，選取案例，然後依照 [[開始] 設定精靈的指示。
    - 如果您選擇 [**進階設定**，按一下 [的箭號來展開**進階設定選項**，然後檢視並設定可用的選項，根據您想要-NPS 功能 RADIUS 伺服器、RADIUS proxy，或兩者。

## <a name="manage-multiple-nps-servers-by-using-the-nps-mmc-snap-in"></a>使用 NPS MMC Snap\ 中管理多個 NPS 伺服器

您可以使用此程序使用 NPS MMC snap\ 在本機伺服器 NPS 和多個的 NPS 遠端伺服器管理。

之前，請先執行下列程序，您必須在本機電腦上，並在遠端電腦上安裝 NPS。

根據網路條件和您使用 NPS MMC snap\ 中管理 NPS 伺服器數目，可能會很慢 MMC snap\ 中的回應。 此外，NPS 伺服器設定流量在網路上的遠端管理工作階段使用傳送 NPS snap\ 中。 請確定您的網路都是安全實際及惡意使用者不擁有的存取權的網路流量。

**管理認證** 

若要完成此程序，您必須是系統管理員群組成員。

### <a name="to-manage-multiple-nps-servers-by-using-the-nps-snap-in"></a>使用 snap\ 中 NPS 管理多個 NPS 伺服器

1. 若要打開 MMC，系統管理員身分執行 Windows PowerShell。 Windows PowerShell 中，輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console 開啟。
2. 在 MMC，在**檔案**功能表中，按一下 [**新增/移除 Snap\ 在**。 **[新增或移除 Snap\ 單元**對話方塊。
3. 在**新增或移除 Snap\ 單元**，請在**可用 snap\ 管理單元]**、向下捲動清單、按一下 [**的網路原則伺服器**，，然後按一下 [**新增**。 **選擇電腦**對話方塊。
4. 在**選擇電腦**，確認**本機電腦 \（的電腦上的此主控台 running\）**已選取，然後按一下 [ **[確定]**。 Snap\ 在本機伺服器 NPS 新增到清單中**選取 snap\ 單元**。
5. 在**新增或移除 Snap\ 單元**，請在**可用 snap\ 管理單元]**，確保**網路原則伺服器**是仍然選取，然後再按一下**新增**。 **選擇電腦**對話方塊再試一次。
6. 在**選擇電腦**，按一下 [**另一部電腦**，然後輸入 IP 位址的完整的網域名稱 \(FQDN\) 遠端 NPS 伺服器您想要使用 snap\ 中 NPS 管理。 或者，您可以按一下**瀏覽]**以仔細 directory 您想要新增的電腦。 按一下**[確定]**。
7. 重複執行步驟 5 和 6 到 NPS snap\ 中新增更多 NPS 伺服器。 完成新增所有 NPS 伺服器您想要管理，請按一下**[確定]**。
8. 若要儲存更新使用 NPS 嵌入式管理單元，按一下 [**檔案**，然後按一下 [**儲存**。 在**另存新檔**對話方塊中，瀏覽至您想要儲存的檔案，輸入您的 Microsoft Management Console \(.msc\) 檔案的名稱，然後按一下硬碟位置**儲存**。 

## <a name="manage-an-nps-server-by-using-remote-desktop-connection"></a>使用遠端桌面連接管理 NPS 伺服器

您可以使用此程序 NPS 遠端伺服器管理使用遠端桌面連接。

使用遠端桌面連接，您可以從遠端管理執行 Windows Server 2016 NPS 伺服器。 您也可以從遠端可以管理 NPS 伺服器執行 Windows 10 或較舊版本的 Windows client 作業系統的電腦。

您可以使用遠端桌面連接到使用兩種方法來管理多個 NPS 伺服器。

1. 排列建立遠端桌面連接到每個 NPS 伺服器。
2. 連接一 NPS 伺服器，請使用遠端桌面，然後使用該伺服器上 NPS MMC 管理其他遠端伺服器。 如需詳細資訊，請查看一節**使用 NPS MMC Snap\ 中管理多個 NPS 伺服器**。

**管理認證** 

若要完成此程序，您必須是 NPS 伺服器上的系統管理員群組成員。

### <a name="to-manage-an-nps-server-by-using-remote-desktop-connection"></a>使用遠端桌面連接管理 NPS 伺服器

1. 在每個 NPS 伺服器您想要管理遠端電腦上，在伺服器管理員中，選取 [**本機伺服器**。 在伺服器管理員詳細資料窗格中，檢視**遠端桌面**設定，然後執行下列其中一項。 
    1. 如果的值**遠端桌面**設定**啟用**，您不需要的一些步驟執行這個程序中。 跳到 [開始] 設定遠端桌面使用者權限來執行「步驟 4。
    2. 如果**遠端桌面**設定**已停用**，按一下 [word**停用**。 **系統屬性**對話方塊在**遠端**索引標籤。
2. 在**遠端桌面**，按一下 [**可讓遠端連接到這部電腦**。 **遠端桌面連接**對話方塊。 執行下列其中一個動作。
    1. 自訂允許網路連接，請按一下**Windows 防火牆進階安全性與**，然後設定允許您想要的設定。 
    2. 若要讓遠端桌面連接，針對所有網路連接的電腦上，按一下 [ **[確定]**。
3. 在**系統屬性**，請在**遠端桌面**，可以選擇是否要讓**允許只有來自電腦執行遠端桌面與網路層級驗證**，，讓您的選擇。
4. 按一下**選取 [使用者]**。 **遠端桌面使用者**對話方塊。
5. 在**遠端桌面使用者**，以授予權限來連接遠端伺服器 NPS，請按一下 [使用者**新增**，然後輸入帳號的使用者名稱。 按一下**[確定]**。
6. 重複執行「步驟 5 為每個您要 NPS server 的遠端存取權限授與的使用者。 當您完成時新增的使用者時，請按一下**[確定]**以關閉 [**遠端桌面使用者**對話方塊和**[確定]**再試一次以關閉 [**系統屬性**] 對話方塊。
7. 若要連接到您所使用的上一個步驟來設定遠端 NPS 伺服器，請按一下**[開始]**，向下捲動排序清單，然後按**Windows 附屬應用程式**，並按一下 [**遠端桌面連接**。 **遠端桌面連接**對話方塊。
8. 在**遠端桌面連接**對話方塊中，在**電腦**，輸入 NPS 伺服器名稱或 IP 位址。 如果您想要的話，請按一下**選項**，設定連接其他選項]，然後按**儲存**儲存連接重複使用。
9. 按一下**連接**，以及出現提示時提供的權限來登入並設定 NPS 伺服器 account 使用者 account 認證。

## <a name="use-netsh-nps-commands-to-manage-an-nps-server"></a>使用 Netsh NPS 命令來管理 NPS 伺服器

顯示及計量] 與稽核使用同時 NPS 並遠端存取服務資料庫設定的驗證，驗證，設定，您可以 Netsh NPS 環境中使用的命令。 使用命令來 Netsh NPS 操作：

- 設定，或重新設定 NPS 伺服器，包括 NPS 的也適用於設定 Windows 介面中使用 NPS 主機的各個層面。
- 匯出一個 NPS 伺服器（來源），包括登錄和 NPS 設定存放區，做為 Netsh 指令碼的設定。
- 使用 Netsh 指令碼，並從來源 NPS 伺服器匯出的設定檔到另一個 NPS 伺服器匯入的設定。

您可以從 Windows Server 2016 命令提示字元或 Windows PowerShell 來執行下列命令。 您也可以執行 netsh nps 命令指令碼與「批次檔案中。

**管理認證** 

若要執行此程序，您必須使用本機電腦上的系統管理員群組成員。

### <a name="to-enter-the-netsh-nps-context-on-an-nps-server"></a>NPS 伺服器上輸入 Netsh NPS 操作

1. 命令提示字元」或「Windows PowerShell 開放。
2. 輸入**netsh**，然後按 ENTER 鍵。
3. 輸入**nps**，然後按 ENTER 鍵。
4. 若要檢視可用的命令的清單，輸入問號 \(?\) 並按下 ENTER。


如需 NPS Netsh 命令，查看[在 Windows Server 2008 的網路原則伺服器 Netsh 命令](https://technet.microsoft.com/library/cc754428(v=ws.10).aspx)，或下載整個[Netsh 技術參考](https://gallery.technet.microsoft.com/Netsh-Technical-Reference-c46523dc?redir=0)從 TechNet 主題館。 此下載是完整網路殼層技術參考適用於 Windows Server 2008 和 Windows Server 2008 R2。 格式不 zip 檔案中的 Windows 協助 \(*.chm\)。 要這些指令，仍然會出現在 Windows Server 2016 和 Windows 10，可讓您使用 netsh 這些的環境中，建議您使用 Windows PowerShell 雖然。

## <a name="use-windows-powershell-to-manage-nps-servers"></a>使用 Windows PowerShell 來管理 NPS 伺服器

您可以使用 Windows PowerShell 命令來管理 NPS 伺服器。 如需詳細資訊，下列 Windows PowerShell 命令參考主題。

- [網路 Windows PowerShell 中的原則 (NPS) 伺服器 Cmdlet](https://technet.microsoft.com/library/jj872739(v=wps.630).aspx)。 您可以使用在 Windows Server 2012 R2 或更新版本作業系統這些 netsh 命令。
- [NPS 模組](https://technet.microsoft.com/itpro/powershell/windows/nps/index)。 您可以在 Windows Server 2016 中使用 netsh 命令。

如需 NPS 管理的詳細資訊，請查看[管理的網路原則 Server (NPS)](nps-manage-top.md)。
