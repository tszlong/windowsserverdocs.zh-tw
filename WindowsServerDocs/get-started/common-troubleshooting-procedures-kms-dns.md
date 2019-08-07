---
title: KMS 和 DNS 問題的常見疑難排解程序
description: ''
ms.topic: article
ms.date: 07/22/2019
ms.technology: server-general
ms.assetid: ''
author: Teresa-Motiv
ms.author: v-tea
ms.localizationpriority: medium
ms.openlocfilehash: 12e3c1fa82a567c43507df2f2ffd72595c3903ba
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68664890"
---
# <a name="common-troubleshooting-procedures-for-kms-and-dns-issues"></a>KMS 和 DNS 問題的常見疑難排解程序

如果下列其中一個或多個條件成立，您可能必須使用其中一些方法：

- 您可以使用大量授權的媒體和大量授權一般產品金鑰來安裝下列其中一個作業系統：&nbsp;
   - Windows Server 2019
   - Windows Server 2016
   - Windows Server 2012 R2
   - Windows Server 2012
   - Windows Server 2008 R2
   - Windows Server 2008
   - Windows 10
   - Windows 8.1
   - Windows 8
- 啟用精靈無法連線至 KMS 主機電腦。

當您嘗試啟動用戶端系統時，啟用精靈會使用 DNS 來尋找執行 KMS 軟體的對應電腦。 如果此精靈查詢 DNS，但找不到 KMS 主機電腦的 DNS 專案，則會回報錯誤。   

<a id="list"></a>請檢閱下列清單，尋找符合您情況的方法：

- 如果您無法安裝 KMS 主機，或如果您無法使用 KMS 啟用，請嘗試[將產品金鑰變更為 MAK](#change-the-product-key-to-an-mak)程序。
- 如果您必須安裝和設定 KMS 主機，請使用[針對要啟用得用戶端設定 KMS 主機](#configure-a-kms-host-for-the-clients-to-activate-against)程序。
- 如果用戶端找不到您現有的 KMS 主機，請使用下列程式針對您的路由組態進行疑難排解。 這些程序會從最簡單到最複雜的方式排列。
  - [確認 DNS 伺服器的基本 IP 連線能力](#verify-basic-ip-connectivity-to-the-dns-server)
  - [確認 KMS 主機設定](#verify-the-configuration-of-the-kms-host)  
  - [判斷路由問題的類型](#determine-the-type-of-routing-issue)
  - [確認 DNS 設定](#verify-the-dns-configuration)
  - [手動建立 KMS SRV 記錄](#manually-create-a-kms-srv-record)
  - [手動將 KMS 主機指派給 KMS 用戶端](#manually-assign-a-kms-host-to-a-kms-client)
  - [將 KMS 主機設定為在多個 DNS 網域中發佈](#configure-the-kms-host-to-publish-in-multiple-dns-domains)

## <a name="change-the-product-key-to-an-mak"></a>將產品金鑰變更為 MAK

如果您無法安裝 KMS 主機，或基於某些原因而無法使用 KMS 啟用，請將產品金鑰變更為 MAK。 如果您從 Microsoft Developer Network (MSDN) 或從 TechNet 下載了 Windows 映像，則媒體底下所列的庫存單位 (SKU) 通常為大量授權媒體，而所提供的產品金鑰為 MAK 金鑰。

若要將產品金鑰變更為 MAK，請遵循下列步驟：

1. 開啟提升權限的 [命令提示字元] 視窗。 若要這麼做，請按 Windows 標誌鍵+X，以滑鼠右鍵按一下 [命令提示字元]  ，然後選取 [以系統管理員身分執行]  。 如果系統提示您輸入系統管理員密碼或進行確認，請輸入密碼或提供確認。
2. 在命令提示字元中執行以下命令：
   ```cmd
    slmgr -ipk xxxxx-xxxxx-xxxxx-xxxxx-xxxxx
   ```
   > [!NOTE]
   > **xxxxx-xxxxx-xxxxx-xxxxx-xxxxx**預留位置代表您的 MAK產品金鑰。  

[返回程式清單。](#list)

## <a name="configure-a-kms-host-for-the-clients-to-activate-against"></a>針對要啟用的用戶端設定 KMS 主機

KMS 啟用要求針對要啟動的用戶端設定 KMS 主機。 如果您的環境中未設定 KMS 主機，請使用適當的 KMS 主機金鑰來安裝並啟動一個。 在網路上設定電腦來裝載 KMS 軟體之後，請發佈網域名稱系統 (DNS) 設定。

如需 KMS 主機設定程序的詳細資訊，請參閱[使用金鑰管理服務進行啟用](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-key-management-service-vamt)及[安裝和設定 VMAT](https://docs.microsoft.com/windows/deployment/volume-activation/install-configure-vamt)。

[返回程式清單。](#list)

## <a name="verify-basic-ip-connectivity-to-the-dns-server"></a>確認 DNS 伺服器的基本 IP 連線能力

使用 ping 命令確認 DNS 伺服器的基本 IP 連線能力。 若要這麼做，請在發生錯誤的 KMS 用戶端和 KMS 主機電腦上，執行下列步驟：

1. 開啟提升權限的 [命令提示字元] 視窗。
1. 在命令提示字元中執行以下命令：
   ```cmd
   ping <DNS_Server_IP_address>
   ```
   > [!NOTE]
   > 如果此命令的輸出不包含 "Reply from" 一詞，您必須先解決網路問題或 DNS 問題，才能使用本文中的其他程序。 如果您無法 ping DNS 伺服器，而需要如何針對 TCP/IP 問題進行疑難排解的詳細資訊，請參閱 [TCP/IP 問題的進階疑難排解](https://docs.microsoft.com/windows/client-management/troubleshoot-tcpip)。

[返回程式清單。](#list)

## <a name="verify-the-configuration-of-the-kms-host"></a>確認 KMS 主機的設定

檢查 KMS 主機伺服器的登錄，以判斷它是否向 DNS 登錄。 根據預設，KMS 主機伺服器會每隔 24小時動態登錄一次 DNS SRV 記錄。 
> [!IMPORTANT]
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/en-us/help/322756)，以免發生問題。  

若要檢查此設定，請遵循下列步驟：
1. 啟動 [登錄編輯程式]。 若要這麼做，請以滑鼠右鍵按一下 [開始]  、選取 [執行]  輸入 **regedit**，然後按 Enter。
1. 找出 **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL** 子機碼，並檢查 **DisableDnsPublishing** 項目的值。 此項目具有下列可能的值：
   - **0** 或未定義 (預設值)：KMS 主機伺服器會每隔 24 小時登錄一次 SRV 記錄。
   - **1**：KMS 主機伺服器不會自動登錄 SRV 記錄。 如果您的實作不支援動態更新，請參閱[手動建立 KMS SRV 記錄](#manually-create-a-kms-srv-record)。  
1. 如果遺漏 **DisableDnsPublishing**項目，請加以建立 (類型為 DWORD)。 如果可接受動態登錄，請將值保持未定義狀態，或將其設定為 **0**。

[返回程式清單。](#list)

## <a name="determine-the-type-of-routing-issue"></a>判斷路由問題的類型

您可以使用下列命令來判斷這是名稱解析問題或 SRV 記錄問題。  

1. 在 KMS 用戶端上，開啟提升權限的 [命令提示字元] 視窗。  
1. 在命令提示字元中執行以下命令：
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > 在此命令中，<KMS_FQDN> 代表 KMS 主機電腦的完整網域名稱 (FQDN)，而 \<port\> 代表 KMS 使用的 TCP 連接埠。  

   如果這些命令解決了問題，這就是 SRV 記錄問題。 您可以使用[手動將 KMS 主機指派給 KMS 用戶端](#manually-assign-a-kms-host-to-a-kms-client)程序中記載的其中一個命令，對其進行疑難排解。  

1. 如果問題持續發生，請執行下列命令：
   ```cmd
   cscript \windows\system32\slmgr.vbs -skms <IP Address>:<port>
   cscript \windows\system32\slmgr.vbs -ato
   ```
   > [!NOTE]
   > 在此命令中，\<IP Address\> 代表 KMS 主機電腦的 IP 位址，而 \<port\> 代表 KMS 使用的 TCP 連接埠。  

   如果這些命令解決了問題，這很可能是名稱解析問題。 如需其他疑難排解資訊，請參閱[驗證 DNS 設定](#verify-the-dns-configuration)程序。

1. 如果這些命令都無法解決問題，請檢查電腦的防火牆設定。 KMS 用戶端與 KMS 主機之間發生的任何啟用通訊都會使用 1688 TCP 通訊埠。 KMS 用戶端和 KMS 主機上的防火牆都必須允許透過連接埠 1688 進行通訊。

[返回程式清單。](#list)

## <a name="verify-the-dns-configuration"></a>確認 DNS 設定

>[!NOTE]
> 除非另有說明，否則請在發生適用錯誤的 KMS 用戶端上執行下列步驟。

1. 開啟提升權限的 [命令提示字元] 視窗。
1. 在命令提示字元中執行以下命令：
   ```cmd
   IPCONFIG /all
   ```
1. 從命令結果中，請記下下列資訊：
   - KMS 用戶端電腦的受指派 IP 位址
   - KMS 用戶端電腦使用的主要 DNS 伺服器 IP 位址
   - KMS 用戶端電腦使用的預設閘道 IP 位址
   - KMS 用戶端電腦使用的 DNS 尾碼搜尋清單
1. 確認 KMS 主機 SRV 記錄已登錄在 DNS 中。 若要這樣做，請執行下列步驟：  
   1. 開啟提升權限的 [命令提示字元] 視窗。
   1. 在命令提示字元中執行以下命令：
      ```cmd
      nslookup -type=all _vlmcs._tcp>kms.txt
      ```
   1. 開啟命令所產生的 KMS.txt 檔案。 這個檔案應該包含一或多個與下列項目類似的項目：
       ```
       _vlmcs._tcp.contoso.com SRV service location:
       priority = 0
       weight = 0
       port = 1688 svr hostname = kms-server.contoso.com
       ```
       > [!NOTE]
       > 在此項目中，contoso.com 代表 KMS 主機的網域。
      1. 確認 KMS 主機的 IP 位址、主機名稱、連接埠和網域。
      1. 如果這些 **_vlmcs** 項目存在，而且它們包含預期的 KMS 主機名稱，請移至[手動將 KMS 主機指派給 KMS 用戶端](#manually-assign-a-kms-host-to-a-kms-client)。  
      > [!NOTE]
      > 如果 [**nslookup**](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup) 命令找到 KMS 主機，並不表示 DNS 用戶端可找到 KMS 主機。 如果 **nslookup** 命令找到 KMS 主機，但您仍然無法使用 KMS 主機進行啟用，請檢查其他 DNS 設定，例如主要 DNS 尾碼和 DNS 尾碼的搜尋清單。
1. 確認主要 DNS 尾碼的搜尋清單包含與 KMS 主機相關聯的 DNS 網域尾碼。 如果搜尋清單不包含這項資訊，請移至[將 KMS 主機設定為在多個 DNS 網域中發佈](#configure-the-kms-host-to-publish-in-multiple-dns-domains)程序。

[返回程式清單。](#list)

## <a name="manually-create-a-kms-srv-record"></a>手動建立 KMS SRV 記錄

若要手動為使用 Microsoft DNS 伺服器的 KMS 主機建立 SRV 記錄，請遵循下列步驟：

1. 在 DNS 伺服器上開啟 [DNS 管理員]。 若要開啟 DNS 管理員，請選取 [開始]  選取 [系統管理工具]  ，然後選取 [DNS]  。
1. 選取您必須在其上建立 SRV 資源記錄的 DNS 伺服器。
1. 在主控台樹狀目錄中，展開 [正向對應區域]  ，以滑鼠右鍵按一下網域，然後選取 [其他新記錄]  。
1. 向下捲動清單，選取 [服務位置 (SRV)]  ，然後選取 [建立記錄]  。
1. 輸入下列資訊：
   - 服務： **_VLMCS**
   - 通訊協定： **_TCP**
   - 連接埠號碼：**1688**
   - 提供這項服務的主機： **&lt;*KMS 主機的 FQDN*&gt;**
1. 完成後，請選取 [確定]  ，然後選取 [完成]  。

若要手動為使用 BIND 9.x 相容 DNS 伺服器的 KMS 主機建立 SRV 記錄，請遵循該 DNS 伺服器的指示，並提供 SRV 記錄的下列資訊：

- 名稱：&nbsp; **_vlmcs._TCP**
- 類型：&nbsp;**SRV**
- 優先順序：**0**
- 權數：**0**
- 連接埠：**1688**
- 主機名稱： **&lt;*KMS 主機的 FQDN 或名稱*&gt;**

> [!NOTE]
> KMS 不會使用 [優先順序]  或 [權數]  值。 不過，記錄必須包含這些項目。

若要設定 BIND 9.x 相容 DNS 伺服器以支援 KMS 自動發佈，請將 DNS 伺服器設定為啟用來自 KMS 主機的資源記錄更新。 例如，將下列一行程式碼新增至 Named.conf 或 Named.conf.local 中的區域定義：

```cmd
allow-update { any; };
```
## <a name="manually-assign-a-kms-host-to-a-kms-client"></a>手動將 KMS 主機指派給 KMS 用戶端

根據預設，KMS 用戶端會使用自動探索程序。 根據此程序，KMS 用戶端會查詢 DNS，以取得用戶端的成員資格區域內已發佈 _vlmcs SRV 記錄的伺服器清單。 DNS 會以隨機順序傳回 KMS 主機的清單。 用戶端會挑選 KMS 主機並嘗試在其上建立工作階段。 如果此嘗試可行，用戶端會快取 KMS 主機的名稱，並嘗試將其使用於下一次更新嘗試。 如果工作階段設定失敗，用戶端會隨機挑選另一個 KMS 主機。 我們強烈建議您使用自動探索程序。  

不過，您可以手動將 KMS 主機指派給特定 KMS 用戶端。 若要這樣做，請執行下列步驟。

1. 在 KMS 用戶端上，開啟提升權限的 [命令提示字元] 視窗。
1. 根據您的實作方式，遵循下列其中一個步驟：
   - 若要使用主機的 FQDN 來指派 KMS 主機，請執行下列命令：
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <KMS_FQDN>:<port>
     ```
   - 若要使用主機的第 4 版 IP 位址來指派 KMS 主機，請執行下列命令：
     ```cmd
     cscript \windows\system32\slmgr.vbs -skms <IPv4Address>:<port>
     ```
    - 若要使用主機的第 6 版 IP 位址來指派 KMS 主機，請執行下列命令：
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <IPv6Address>:<port>
      ```
    - 若要使用主機的 NETBIOS 名稱來指派 KMS 主機，請執行下列命令：
      ```cmd
      cscript \windows\system32\slmgr.vbs -skms <NETBIOSName>:<port>
      ```
   - 若要在 KMS 用戶端上還原為自動探索，請執行下列命令：
     ```cmd
     cscript \windows\system32\slmgr.vbs -ckms
     ```
     > [!NOTE]
     > 這些命令會使用下列預留位置：
     >- **<KMS_FQDN>** 代表 KMS 主機電腦的完整網域名稱 (FQDN)
     >- **\<IPv4Address\>** 代表 KMS 主機電腦的第 4 版 IP 位址
     >- **\<IPv6Address\>** 代表 KMS 主機電腦的第 6 版 IP 位址
     >- **\<\> NETBIOSName** 代表 KMS 主機電腦的 NETBIOS 名稱
     >- **\<port\>** 代表 KMS 使用的 TCP 連接埠。  

## <a name="configure-the-kms-host-to-publish-in-multiple-dns-domains"></a>將 KMS 主機設定為在多個 DNS 網域中發佈

> [!IMPORTANT]
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/help/322756)，以免發生問題。

如[手動將 KMS 主機指派給 KMS 用戶端](#manually-assign-a-kms-host-to-a-kms-client)所述，KMS 用戶端通常會使用自動探索程序來識別 KMS 主機。 此程序要求 _vlmcs SRV 記錄必須可在 KMS 用戶端電腦的 DNS 區域中取得。 DNS 區域會對應至電腦的主要 DNS 尾碼或下列其中一項：
- 若為已加入網域的電腦，則為 DNS 系統所指派的電腦網域 (例如 Active Directory Domain Services (AD DS) DNS)。
- 若為工作群組電腦，這是由動態主機設定通訊協定 (DHCP) 指派的電腦網域。 如要求建議 (RFC) 2132 中所定義，此網域名稱是由代碼值為 15 的選項所定義。

根據預設，KMS 主機會在對應至 KMS 主機電腦網域的 DNS 區域中登錄其 SRV 記錄。 例如，假設 KMS 主機加入 contoso.com 網域。 在此案例中，KMS 主機會在 contoso.com DNS 區域下登錄其 _vmlcs SRV 記錄。 因此，此記錄會將服務識別為 VLMCS._TCP.CONTOSO.COM。

如果 KMS 主機和 KMS 用戶端使用不同的 DNS 區域，您必須將 KMS 主機設定為在多個 DNS 網域中自動發佈其 SRV 記錄。 若要這樣做，請執行下列步驟：

1. 在 KMS 主機上，啟動 [登錄編輯程式]。 
1. 找出而後選取 **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SL** 子機碼。
1. 在 [詳細資料]  窗格中，以滑鼠右鍵按一下空白區域，選取 [新增]  ，然後選取 [多字串值]  。
1. 針對新項目的名稱，輸入 **DnsDomainPublishList**。
1. 以滑鼠右鍵按一下新的 [DnsDomainPublishList]  項目，然後選取 [修改]  。
1. 在 [編輯多字串]  對話方塊中，輸入 KMS 在個別行上發佈的每個 DNS 網域尾碼，然後選取 [確定]  。
   > [!NOTE]
   > 對於 Windows Server 2008 R2，[DnsDomainPublishList]  的格式不同。 如需詳細資訊，請參閱《大量啟用技術參考指南》。
1. 使用 [服務] 系統管理工具來重新開機軟體授權服務。 此作業會建立 SRV 記錄。
1. 使用典型方法，確認 KMS 用戶端可連絡您所設定的 KMS 主機。 確認 KMS 用戶端可依名稱和 IP 位址正確地識別 KMS 主機。 如果上述任一項驗證失敗，請調查此 DNS 用戶端解析程式問題。
1. 若要清除 KMS 用戶端上任何先前快取的 KMS 主機名稱，請在 KMS 用戶端上開啟提升權限的 [命令提示字元] 視窗，然後執行下列命令：
   ```cmd
   cscript C:\Windows\System32\slmgr.vbs -ckms
   ```
