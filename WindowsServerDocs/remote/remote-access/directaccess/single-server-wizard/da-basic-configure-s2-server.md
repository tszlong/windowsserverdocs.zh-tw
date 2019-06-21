---
title: 步驟 2 設定基本 DirectAccess 伺服器
description: 本主題是指南部署單一 DirectAccess 伺服器使用取得啟動精靈的 Windows Server 2016 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82bf5fed-93b3-4fa6-8e71-522146eccdb1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5bd248e36c316b11ea5e272707b75624d73dc49a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283422"
---
# <a name="step-2-configure-the-basic-directaccess-server"></a>步驟 2 設定基本 DirectAccess 伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

本主題說明如何設定基本 DirectAccess 部署所需的用戶端與伺服器設定。 開始部署步驟之前，請確定您已完成中所述的規劃步驟[規劃基本 DirectAccess 部署](Plan-a-Basic-DirectAccess-Deployment.md)。  
  
|工作|描述|  
|----|--------|  
|安裝遠端存取角色|安裝「遠端存取」角色。|  
|使用開始使用精靈設定 DirectAccess|新的開始使用精靈大幅簡化設定流程。 這個精靈讓 DirectAccess 的複雜度消失，只要幾個步驟，就可以自動安裝完成。 這個精靈可以自動設定 Kerberos Proxy，這樣就不必部署內部 PKI，讓系統管理員自然完成整個程序。|  
|更新用戶端的 DirectAccess 設定|如果想收到 DirectAccess 設定，用戶端必須在連接內部網路的時候更新群組原則。|  
  
> [!NOTE]  
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Role"></a>安裝遠端存取角色  
若要部署「遠端存取」，您必須在將組織中將做為「遠端存取」伺服器的伺服器上安裝「遠端存取」角色。  
  
#### <a name="to-install-the-remote-access-role"></a>安裝「遠端存取」角色  
  
1.  在遠端存取伺服器上，在 [伺服器管理員] 主控台中，在**儀表板**，按一下**新增角色及功能**。  
  
2.  按 [下一步]  3 次，進入伺服器角色選擇畫面。  
  
3.  在 [選取伺服器角色]  對話方塊中，選取 [遠端存取]  ，然後按 [下一步]  。  
  
4.  在 [選取功能]  對話方塊中，按 [下一步]  。  
  
5.  按一下 **下一步**，然後在**選取角色服務**對話方塊中，按一下  **DirectAccess 和 VPN (RAS)** 核取方塊。  
  
6.  按一下 [**將功能加入**，按一下**下一步]** ，然後按一下**安裝**。  
  
7.  在 [安裝進度]  對話方塊中，確認安裝成功，然後按一下 [關閉]  。  
  
![Windows PowerShell](../../../media/Step-2-Configure-the-DirectAccess-Server/PowerShellLogoSmall.gif)***<em>Windows PowerShell 對等的命令</em>***  
  
下列 Windows PowerShell cmdlet 安裝遠端存取角色： 

1. 以系統管理員身分開啟 PowerShell。

2. 安裝遠端存取功能：

   ```  
   Install-WindowsFeature RemoteAccess   
   ```  

3. 重新啟動電腦：

   ```
   Restart-Computer
   ```
   
4. 安裝遠端存取 」 PowerShell:

   ```
   Install-WindowsFeature RSAT-RemoteAccess-PowerShell
   ```



  
## <a name="configure-directaccess-with-the-getting-started-wizard"></a>使用開始使用精靈設定 DirectAccess  
  
#### <a name="to-configure-directaccess-using-the-getting-started-wizard"></a>使用開始使用精靈設定 DirectAccess  
  
1.  在伺服器管理員中，按一下 [工具]  ，然後按一下 [遠端存取管理]  。  
  
2.  在遠端存取管理主控台中，選取 角色服務，在左側的導覽窗格中，設定，然後按一下**執行開始使用精靈**。  
  
3.  按一下 [僅部署 DirectAccess]  。  
  
4.  選取網路設定拓樸，然後輸入遠端存取用戶端會連接的公用名稱。 按一下 [下一步]  。  
  
    > [!NOTE]  
    > 根據預設值，開始使用精靈會將 WMI 篩選器套用至用戶端設定 GPO，進而將 DirectAccess 部署至網域內的所有膝上型或筆記型電腦。  
  
5.  按一下 **[完成]** 。  
  
6.  因為這個部署沒有使用 PKI，如果找不到憑證，精靈會自動為 IP-HTTPS 和網路位置伺服器佈建自我簽署憑證，而且會自動啟用 Kerberos Proxy。 精靈也會啟用 NAT64 和 DNS64，以便轉換 IPv4 環境使用的通訊協定。 精靈套用設定成功完成之後，按一下 [關閉]  。  
  
7.  在 [遠端存取管理] 主控台的主控台樹狀目錄中，按一下 [操作狀態]  。 等候所有監視器狀態顯示成「工作中」為止。 在 [監視] 的 [工作] 窗格中，按一下 [重新整理]  ，定期更新畫面。  
  
## <a name="update-clients-with-the-directaccess-configuration"></a>更新用戶端的 DirectAccess 設定  
  
#### <a name="to-update-directaccess-clients"></a>更新 DirectAccess 用戶端  
  
1.  系統管理員身分開啟 PowerShell。  
  
2.  在 PowerShell 視窗，輸入 **gpupdate**，然後按 **ENTER**。  
  
3.  等候電腦原則更新成功完成為止。  
  
4.  輸入 **Get-DnsClientNrptPolicy**，然後按 **ENTER** 鍵  
  
    隨即顯示 DirectAccess 的名稱解析原則表格 (NRPT) 項目。 請注意，電腦會顯示 NLS 伺服器豁免。 開始使用精靈會為 DirectAccess 伺服器自動建立這個 DNS 項目，然後提供相關的自我簽署憑證，讓 DirectAccess 伺服器可以充當網路位置伺服器的功用。  
  
5.  輸入 **Get-NCSIPolicyConfiguration**，然後按 **ENTER** 鍵 隨即顯示精靈部署的網路連線狀態指示器設定。 請注意 DomainLocationDeterminationURL 的值。 只要這個網路位置伺服器 URL 可以連線，用戶端就會判斷它是在公司網路裡面，而且不會套用 NRPT 設定。  
  
6.  輸入 **Get-DAConnectionStatus**，然後按 **ENTER** 鍵 因為用戶端可以連線網路位置伺服器 URL，所以狀態會顯示成 [本機連線]  。  
  
## <a name="BKMK_Links"></a>上一個步驟  
  
-   [步驟 1：設定 DirectAccess 基礎結構](Step-1-Configure-the-DirectAccess-Infrastructure.md)  
  
## <a name="next-step"></a>後續步驟  
  
-   [步驟 3： 確認基本 DirectAccess 部署](da-basic-configure-s3-verify.md)  
  


