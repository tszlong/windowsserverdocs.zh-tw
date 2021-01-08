---
title: 步驟2設定遠端存取服務器
description: 瞭解如何設定遠端系統管理 DirectAccess 用戶端所需的用戶端和伺服器設定。
manager: brianlic
ms.topic: article
ms.assetid: c0257b98-5633-4264-9df6-b6ffae80592c
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 7d7b2a29ef22f8768861eb456e0ded2c0e9a9432
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040037"
---
# <a name="step-2-configure-the-remote-access-server"></a>步驟2設定遠端存取服務器

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何設定遠端系統管理 DirectAccess 用戶端所需的用戶端和伺服器設定。 開始部署步驟之前，請確定您已完成 [步驟2規劃遠端存取部署](../plan/Step-2-Plan-the-Remote-Access-Deployment.md)中所述的規劃步驟。

|工作|描述|
|----|--------|
|安裝遠端存取角色|安裝「遠端存取」角色。|
|設定部署類型|將部署類型設定為 DirectAccess 與 VPN、僅 DirectAccess 或僅 VPN。|
|設定 DirectAccess 用戶端|在「遠端存取」伺服器設定包含 DirectAccess 用戶端的安全性群組。|
|設定遠端存取伺服器|設定遠端存取服務器設定。|
|設定基礎結構伺服器|設定組織中所使用的基礎結構伺服器。|
|設定應用程式伺服器|將應用程式伺服器設定為需要驗證和加密。|
|設定摘要和替代 GPO|檢視「遠端存取」設定摘要，並視需要修改 GPO。|

> [!NOTE]
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="install-the-remote-access-role"></a><a name="BKMK_Role"></a>安裝遠端存取角色
您必須在組織中的伺服器上安裝遠端存取角色，作為遠端存取服務器。

#### <a name="to-install-the-remote-access-role"></a>安裝「遠端存取」角色

### <a name="to-install-the-remote-access-role-on-directaccess-servers"></a>在 DirectAccess 伺服器上安裝遠端存取角色

1.  在 DirectAccess 伺服器的伺服器管理員主控台中，按一下 [ **儀表板**] 中的 [ **新增角色及功能**]。

2.  按 [下一步] 3 次，進入伺服器角色選擇畫面。

3.  在 [ **選取伺服器角色** ] 對話方塊中，選取 [ **遠端存取**]，然後按 **[下一步]**。

4.  按三次 **[下一步]** 。

5.  在 [ **選取角色服務** ] 對話方塊中，選取 [ **DirectAccess 和 VPN] (RAS)** 然後按一下 [ **新增功能**]。

6.  依序選取 [ **路由**]、[ **Web 應用程式 Proxy**]、[ **新增功能** **] 和 [下一步]**。

7. 按 [下一步]，然後按一下 [安裝]。

8.  在 [安裝進度] 對話方塊中，確認安裝成功，然後按一下 [關閉]。

![Windows PowerShell ](../../../../media/Step-2-Configure-the-Remote-Access-Server/PowerShellLogoSmall.gif) * *_<em>Windows PowerShell 對等命令</em>_* _

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```
Install-WindowsFeature RemoteAccess -IncludeManagementTools
```

## <a name="configure-the-deployment-type"></a><a name="BKMK_Deploy"></a>設定部署類型
有三個選項可用來從遠端存取管理主控台部署遠端存取：

-   DirectAccess 與 VPN

-   僅 DirectAccess

-   僅 VPN

> [!NOTE]
> 本指南使用範例程式中僅限 DirectAccess 的部署方法。

#### <a name="to-configure-the-deployment-type"></a>設定部署類型

1.  在 [遠端存取] 伺服器上，開啟 [遠端存取管理] 主控台：在 [_ *開始*] 畫面上輸入，輸入 **Remote access management console**，然後按 enter。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。

2.  在 [遠端存取管理] 主控台的中央窗格中，按一下 [執行遠端存取安裝精靈]。

3.  在 [ **設定遠端存取** ] 對話方塊中，選取 [DIRECTACCESS 和 vpn]、[僅 directaccess] 或 [僅 vpn]。

## <a name="configure-directaccess-clients"></a><a name="BKMK_Clients"></a>設定 DirectAccess 用戶端
若要佈建用戶端電腦以使用 DirectAccess，它必須屬於所選取的安全性群組。 設定 DirectAccess 之後，會布建安全性群組中的用戶端電腦，以接收 DirectAccess 群組原則物件 (Gpo) 以進行遠端系統管理。

#### <a name="to-configure-directaccess-clients"></a>設定 DirectAccess 用戶端

1.  在 [遠端存取管理主控台] 中間窗格的 [步驟 1 遠端用戶端] 區域中，按一下 [設定]。

2.  在 [DirectAccess 用戶端安裝] 頁面的 [ **部署案例** ] 頁面上，按一下 [ **僅部署 DirectAccess 進行遠端系統管理**]，然後按 **[下一步]**。

3.  在 [選取群組] 頁面上，按一下 [新增]。

4.  在 [ **選取群組** ] 對話方塊中，選取包含 DirectAccess 用戶端電腦的安全性群組，然後按 **[下一步]**。

5.  在 [網路連線助理] 頁面上：

    -   在資料表中，新增將用來判斷內部網路連線的資源。 如果沒有設定任何其他資源，將會自動建立預設 Web 探查。 設定 web 探查位置以決定商業網路的連線時，請確定您已設定至少一個以 HTTP 為基礎的探查。 僅設定 ping 探查並不足夠，而且可能會導致無法正確判斷線上狀態。 這是因為 ping 已豁免于 IPsec。 因此，ping 並不確定已正確建立 IPsec 通道。

    -   新增「技術服務人員」電子郵件地址，以允許使用者在遇到連線問題時傳送資訊。

    -   提供 DirectAccess 連線的易記名稱。

    -   視需要選取 [允許 DirectAccess 用戶端使用本機名稱解析] 核取方塊。

        > [!NOTE]
        > 啟用本機名稱解析時，執行 NCA 的使用者可以使用 DirectAccess 用戶端電腦上設定的 DNS 伺服器來解析名稱。

6.  按一下 [完成] 。

## <a name="configure-the-remote-access-server"></a><a name="BKMK_Server"></a>設定遠端存取伺服器
若要部署遠端存取，您需要使用下列內容來設定將作為遠端存取服務器的伺服器：

1.  正確的網路介面卡

2.  遠端存取服務器的公用 URL，用戶端電腦可以 (ConnectTo 位址連接到該伺服器) 

3.  具有符合 ConnectTo 位址之主體的 IP-HTTPS 憑證

4.  IPv6 設定

5.  用戶端電腦驗證

#### <a name="to-configure-the-remote-access-server"></a>設定遠端存取伺服器

1.  在 [遠端存取管理主控台] 中間窗格的 [步驟 2 遠端存取伺服器] 區域中，按一下 [設定]。

2.  在 [遠端存取伺服器安裝精靈] 的 [網路拓撲] 頁面上，按一下將在您組織中使用的部署拓撲。 在 [輸入用戶端用於連線至遠端存取伺服器的公用名稱或 IPv4 位址]、輸入部署的公用名稱 (此名稱符合 IP-HTTPS 憑證的主體名稱，例如，edge1.contoso.com)，然後按 [下一步]。

3.  在 [ **網路介面卡** ] 頁面上，嚮導會自動偵測：

    -   部署中網路的網路介面卡。 如果精靈沒有偵測到正確的網路介面卡，請手動選取正確的介面卡。

    -   IP-HTTPS 憑證。 這是以您在嚮導的上一個步驟中設定之部署的公用名稱為基礎。 如果 wizard 沒有偵測到正確的 IP-HTTPS 憑證，請按一下 **[流覽]** 以手動方式選取正確的憑證。

4.  按 [下一步]  。

5.  在 [ **首碼** 設定] 頁面上 (只有在內部網路中偵測到 ipv6 時才會顯示此頁面) 中，嚮導會自動偵測內部網路上所使用的 ipv6 設定。 如果您的部署需要額外的首碼，請設定內部網路的 IPv6 首碼、要指派給 DirectAccess 用戶端電腦的 IPv6 首碼，以及要指派給 VPN 用戶端電腦的 IPv6 首碼。

6.  在 [驗證] 頁面上：

    -   針對多站台和雙因素驗證部署，您必須使用電腦憑證驗證。 選取 [ **使用電腦憑證** ] 核取方塊以使用電腦憑證驗證，並選取 IPsec 根憑證。

    -   若要讓執行 Windows 7 的用戶端電腦透過 DirectAccess 進行連線，請選取 [ **啟用 windows 7 用戶端電腦透過 directaccess 進行連線]** 核取方塊。 在這種部署中，您必須也使用電腦憑證驗證。

7.  按一下 [完成] 。

## <a name="configure-the-infrastructure-servers"></a><a name="BKMK_Infra"></a>設定基礎結構伺服器
若要設定遠端存取部署中的基礎結構伺服器，您必須設定下列各項：

-   網路位置伺服器

-   DNS 設定，包括 DNS 尾碼搜尋清單

-   遠端存取未自動偵測到的任何管理伺服器

#### <a name="to-configure-the-infrastructure-servers"></a>設定基礎結構伺服器

1.  在 [遠端存取管理主控台] 中間窗格的 [步驟 3 基礎結構伺服器] 區域中，按一下 [設定]。

2.  在 [基礎結構伺服器安裝精靈] 的 [網路位置伺服器] 頁面上，按一下與部署中網路位置伺服器的位置對應的選項。

    -   如果網路位置伺服器位於遠端網頁伺服器上，請輸入 URL，然後按一下 [ **驗證** ]，再繼續進行操作。

    -   如果網路位置伺服器位於「遠端存取」伺服器上，請按一下 [瀏覽] 來找出相關的憑證，然後按 [下一步]。

3.  在 [ **DNS** ] 頁面的表格中，輸入將套用為「名稱解析原則」表格 (NRPT) 豁免的其他名稱尾碼。 選取本機名稱解析選項，然後按 [下一步]。

4.  在 [ **DNS 尾碼搜尋清單** ] 頁面上，遠端存取服務器會自動偵測部署中的網域尾碼。 使用 [ **新增** ] 和 [ **移除** ] 按鈕，即可建立您想要使用的網域尾碼清單。 若要新增網域尾碼，請在 [新尾碼] 中輸入尾碼，然後按一下 [新增]。 按 [下一步]  。

5.  在 [ **管理** ] 頁面上，新增系統不會自動偵測到的管理伺服器，然後按 **[下一步]**。 遠端存取會自動新增網域控制站和 Configuration Manager 伺服器。

6.  按一下 [完成] 。

## <a name="configure-application-servers"></a><a name="BKMK_App"></a>設定應用程式伺服器
在完整的遠端存取部署中，設定應用程式伺服器是選擇性的工作。 在此案例中，若要遠端系統管理 DirectAccess 用戶端，則不會使用應用程式伺服器，而此步驟會呈現灰色，表示它不在使用中。 按一下 **[完成]** 以套用設定。

## <a name="configuration-summary-and-alternate-gpos"></a><a name="BKMK_GPO"></a>設定摘要和替代 GPO
當「遠端存取」設定完成時，會顯示 [遠端存取檢閱]。 您可以檢閱先前選取的所有設定，包括：

-   **GPO 設定**

    系統會列出 DirectAccess 伺服器 GPO 名稱與用戶端 GPO 名稱。 您可以按一下 [ **Gpo 設定**] 標題旁的 [**變更**] 連結，以修改 gpo 設定。

-   **遠端用戶端**

    會顯示 DirectAccess 用戶端設定，包括安全性群組、連線能力檢查器，以及 DirectAccess 連線名稱。

-   **遠端存取服務器**

    會顯示 DirectAccess 設定，包括公用名稱和位址、網路介面卡設定和憑證資訊。

-   **基礎結構伺服器**

    此清單包含網路位置伺服器 URL、DirectAccess 用戶端所使用的 DNS 尾碼，以及管理伺服器資訊。

## <a name="see-also"></a><a name="BKMK_Links"></a>另請參閱

-   [步驟 3：檢查部署](Step-3-Verify-the-Deployment_2.md)




