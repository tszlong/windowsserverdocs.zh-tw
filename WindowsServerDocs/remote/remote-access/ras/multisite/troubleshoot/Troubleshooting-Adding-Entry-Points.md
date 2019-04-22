---
title: 疑難排解新增進入點
description: 本主題是本指南的一部分部署多部遠端存取伺服器在 Windows Server 2016 中的多站台部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dcc1037f-1a65-4497-99e6-0df9aef748a8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d4dfd37e2e8d87dafe6de4e03caf7464e3f0ee6c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820459"
---
# <a name="troubleshooting-adding-entry-points"></a>疑難排解新增進入點

>適用於：Windows Server （半年通道），Windows Server 2016

本主題包含與 `Add-DAEntryPoint` 命令問題有關的疑難排解資訊。 若要確認您所收到的錯誤與新增進入點有關，請檢查 Windows 事件記錄檔中的事件識別碼 10067。  
  
## <a name="missing-remoteaccessserver-parameter"></a>缺少 RemoteAccessServer 參數  
**收到的錯誤**。 您必須提供 RemoteAccessServer 參數的值。  
  
**原因**  
  
將新進入點新增到多站台部署時，您必須指定參數 *RemoteAccessServer*，這是您要加入做為新進入點的伺服器名稱。  
  
**解決方法**  
  
執行命令，並確定將 *RemoteAccessServer* 參數指定成要加入做為進入點的伺服器名稱。  
  
## <a name="remote-access-is-not-configured"></a>未設定遠端存取  
**收到的錯誤**。 在 < 伺服器名稱 > 上未設定遠端存取。 請指定屬於多站台部署之伺服器的名稱。  
  
**原因**  
  
*ComputerName* 參數指定的電腦或執行命令的電腦，沒有設定遠端存取。  
  
當加入新項目移到站台部署時，您必須指定兩個參數：*ComputerName*並*RemoteAccessServer*。 *ComputerName* 是已經屬於多站台部署的伺服器名稱，*RemoteAccessServer* 則是您要加入做為新進入點的伺服器名稱。 如果您從屬於多站台部署的電腦執行，就不需要 ComputerName 參數。  
  
**解決方法**  
  
執行命令，並確定將 *ComputerName* 參數指定成已設定為屬於多站台部署的伺服器名稱，或者從屬於多站台部署的電腦執行命令。  
  
## <a name="multisite-not-enabled"></a>未啟用多站台  
**收到的錯誤**。 您必須啟用多站台的部署，才能執行這項作業。 請使用 `Enable-DAMultiSite` Cmdlet 進行啟用。  
  
**原因**  
  
*ComputerName* 參數指定的伺服器未啟用多站台。 若要在遠端存取部署新增進入點，必須先啟用多站台。  
  
**解決方法**  
  
使用 `Enable-DaMultiSite` Cmdlet 啟用多站台。 如需詳細資訊，請參閱[部署多站台遠端存取](https://technet.microsoft.com/library/hh831664.aspx)。  
  
## <a name="ipv6-prefix-issues"></a>IPv6 首碼問題  
  
-   **問題 1**  
  
    **收到的錯誤**。 IPv6 已部署在內部網路中，但您尚未指定用戶端 IPv6 首碼。  
  
    **原因**  
  
    IPv6 部署在公司網路上，需要 IP-HTTPS 首碼。 但是，*ClientIPv6Prefix* 參數沒有指定新進入點的首碼。  
  
    **解決方法**  
  
    1.  為新進入點指派唯一的 IP-HTTPS 首碼，並確定要傳送到此首碼 IP 位址的封包將會路由至您正在新增的伺服器。  
  
    2.  執行 `Add-DAEntryPoint` Cmdlet，並在 *ClientIPv6Prefix* 參數指定 IP-HTTPS 首碼。  
  
-   **問題 2**  
  
    **收到的錯誤**。 用戶端 IPv6 首碼已經由其他進入點的使用中。 請指定其他值。  
  
    **原因**  
  
    *ClientIPv6Prefix* 參數指定的 IP-HTTPS 首碼已被其他進入點使用。  
  
    **解決方法**  
  
    1.  為新進入點指派唯一的 IP-HTTPS 首碼，並確定要傳送到此首碼 IP 位址的封包將會路由至您正在新增的伺服器。  
  
    2.  執行 `Add-DAEntryPoint` Cmdlet，並在 *ClientIPv6Prefix* 參數指定 IP-HTTPS 首碼。  
  
## <a name="connectto-address"></a>ConnectTo 位址  
**收到的錯誤**。 DirectAccess 用戶端在 RemoteAccess 伺服器連接的位址 （< connectto 位址 >） 是網路位置伺服器位址相同。 請指定其他值。  
  
**原因**  
  
ConnectTo 位址與網路位置伺服器位址相同。  
  
**解決方法**  
  
ConnectTo 位址應該可以透過網際網路解析，以便讓用戶端電腦透過 IP-HTTPS 連線。 網路位置伺服器位址應該可以透過公司網路解析，但不可以透過網際網路解析。 請確定網路位置伺服器與 ConnectTo 位址不同。 選取其他位址，然後再試一次。  
  
## <a name="directaccess-or-vpn-already-installed"></a>已安裝 DirectAccess 或 VPN  
**收到的錯誤**。 < 伺服器名稱 > 的伺服器上偵測到 VPN 安裝。 請指定其他未安裝遠端存取的伺服器，或從伺服器中移除 VPN 設定。  
  
或  
  
伺服器 < 伺服器名稱 > 上已安裝遠端存取。 請指定未執行 DirectAccess 的其他伺服器，或移除伺服器中的現有 DirectAccess 設定。  
  
**原因**  
  
新進入點已設定 DirectAccess 或 VPN。 您不能在多站台部署中新增設定好的進入點。  
  
**解決方法**  
  
若要在多站台部署中新增伺服器，您必須在伺服器上安裝遠端存取角色，但不應設定 DirectAccess 和 VPN。  
  
執行命令，並確認 *RemoteAccessServer* 參數指定的伺服器沒有設定 DirectAccess 或 VPN。  
  
## <a name="ipsec-root-certificate"></a>IPsec 根憑證  
**收到的錯誤**。 伺服器 < 伺服器名稱 > 上找不到設定的 IPsec 根憑證。  
  
**原因**  
  
在您嘗試新增到部署的伺服器上，找不到發出電腦憑證的根或中繼憑證授權單位 (CA) 憑證。  
  
**解決方法**  
  
在 [遠端存取管理] 主控台中，按一下 [步驟 2：遠端存取伺服器] 的 [編輯]，然後在 [驗證] 頁面下的 [使用電腦憑證]，確定選取的憑證是有效的。 如果是有效的憑證，確定該憑證位於您要新增之伺服器可信任的根 CA 下，然後再試一次。  
  
> [!NOTE]  
> 憑證必須是相同的憑證且有相同的指紋。  
  
如果是無效的憑證，請選取在所有遠端存取伺服器上設定為可信任的根 CA 之有效憑證。  
  
## <a name="mixing-ipv6-and-ipv4-entry-points"></a>混合 IPv6 和 IPv4 進入點  
第一次安裝 DirectAccess 時，會檢查內部網路介面卡，以判斷網路是僅包含 IPv4 位址 (僅支援 IPv4 的網路)、包含 IPv6 和 IPv4 位址，或僅包含 IPv6 位址 (僅支援 IPv6 的網路)。 這項資訊可用來判斷部署類型 (僅 IPv4、IPv6+IPv4 或僅 IPv6)。  
  
-   **問題 1**  
  
    **收到的警告**。 要新增之遠端存取伺服器設定 IPv4 和 IPv6 位址。 這是僅 IPv4 部署，因此遠端存取將會略過 IPv6 位址。  
  
    **原因**  
  
    最初安裝這個部署時，偵測到的內部網路為僅支援 IPv4 的網路。 在多站台部署中，會假設不同進入點位於不同的子網路，且有不同的特性。 因此，雖然部署設定為僅 IPv4 部署，仍可以包含位於 IPv6+IPv4 子網路的進入點。 不過，雖然的進入點會新增至部署中，DirectAccess 會忽略新進入點內部介面上設定的 IPv6 位址。  
  
    **解決方法**  
  
    如果整個內部網路使用 IPv6 和 IPv4 位址進行設定，請考慮移到 IPv6+IPv4 部署以充分利用 IPv6 技術。 請參閱中的"IPv6 + IPv4 的公司網路有轉換從純 IPv4"[步驟 3:規劃多站台部署](assetId:///19d49dbf-1786-47bb-ab97-f0458c53d91d)。  
  
-   **問題 2**  
  
    **收到的錯誤**。 此多站台部署中的遠端存取伺服器的內部網路介面卡使用 IPv4 位址設定。 您新增的進入點也必須設定為使用內部網路介面卡上的 IPv4 位址。  
  
    **原因**  
  
    最初安裝這個部署時，偵測到的內部網路為僅支援 IPv4 的網路。 遠端存取偵測到您嘗試新增的進入點使用僅 IPv6 位址設定內部網路。 這在僅 IPv4 部署是不允許的。  
  
    **解決方法**  
  
    如果整個網路已經使用 IPv6 位址進行設定，您應該移到 IPv6+IPv4 或僅 IPv6 部署。 請參閱 「 多站台的遠端存取部署時，方案轉換為 IPv6 」。  
  
-   **問題 3**  
  
    **收到的錯誤**。 此進入點位於 IPv4 網路，但先前的進入點位於 IPv6 網路。 請先將此進入點連線至 IPv6 網路，然後再將它新增至相同的多站台部署。  
  
    **原因**  
  
    最初安裝這個部署時，偵測到的內部網路是 IPv6+IPv4 或僅 IPv6。 而且偵測到您嘗試新增的新進入點內部網路只設定 IPv4 位址。 這在 IPv6+IPv4 或僅 IPv6 部署是不允許的。  
  
    **解決方法**  
  
    使用 IPv6 位址設定新進入點，然後將這個進入點新增到多站台部署。  
  
-   **問題 4**  
  
    **收到的警告**。 未設定遠端存取伺服器上的內部網路介面卡的 IPv4 位址。 將不會在此伺服器上設定 DNS64 和 NAT64。 DirectAccess 用戶端只能存取 IPv6 內部伺服器。  
  
    **原因**  
  
    最初安裝這個部署時，偵測到的內部網路是 IPv6+IPv4。 在這個部署模式中，已啟用 DNS64 和 NAT64 讓用戶端電腦存取在內部網路上只使用 IPv4 位址設定的電腦。  
  
    新增進入點時，遠端存取偵測到新電腦上的內部介面只有 IPv6 位址。 若要設定 DNS64 和 NAT64，需要有 IPv4 位址才能將封包從遠端存取伺服器路由至僅 IPv4 電腦。 由於新電腦上沒有這種 IP，將不會在遠端存取伺服器設定 NAT64 和 DNS64。 因此，使用這個進入點透過 DirectAccess 存取公司網路的用戶端電腦將無法在內部網路存取僅 IPv4 伺服器。 如需如何轉換為 IPv6 + IPv4 網路或僅支援 IPv6 的網路資訊，請參閱 「 多站台的遠端存取部署時，方案轉換為 IPv6 」。  
  
    **解決方法**  
  
    將 IPv4 位址新增到新的遠端存取伺服器，以確保 DNS64 和 NAT64 正常運作。  
  
## <a name="domain-issues-with-the-servergponame"></a>ServerGpoName 網域問題  
  
-   **問題 1**  
  
    **收到的錯誤**。 < 伺服器 gpo&gt > ServerGpoName 參數中指定的網域不存在。 改為指定的網域 < 網域名稱 >。  
  
    **原因**  
  
    找不到系統管理員傳送屬於伺服器 GPO 名稱的網域名稱。  
  
    **解決方法**  
  
    請確定您已經輸入正確的網域名稱。 如果網域名稱的拼字正確，請使用完整網域名稱 (FQDN) 再試一次。  
  
-   **問題 2**  
  
    **收到的錯誤**。 伺服器 GPO 必須位於遠端存取伺服器網域。 ServerGpoName 參數中指定的網域 < 網域名稱 >。  
  
    **原因**  
  
    伺服器 GPO 的網域與遠端存取伺服器所屬的網域不同。  
  
    **解決方法**  
  
    伺服器 GPO 應該要位於與遠端存取伺服器相同的網域。 使用伺服器 GPO 的伺服器網域名稱，然後再試一次。  
  
## <a name="split-brain-dns"></a>拆分式 DNS  
**收到的警告**。 DNS 尾碼 < g > 的 NRPT 項目包含用戶端電腦連線到遠端存取伺服器的公用名稱。 新增名稱 < connectto 位址 > 做為 NRPT 的豁免。  
  
**原因**  
  
您使用拆分式 DNS。 若要讓用戶端使用 IP-HTTPS 進行連線，您應該確定選取的 ConnectTo 位址是 NRPT 規則的豁免。  
  
**解決方法**  
  
如果您使用多站台部署，請確定不同進入點的所有 ConnectTo 位址都是 NRPT 規則的豁免。  
  
若要在 NRPT 規則豁免位址：  
  
1.  在 [遠端存取管理] 主控台中，按一下 [步驟 3 基礎結構伺服器] 下的 [編輯]。  
  
2.  在 [基礎結構伺服器安裝] 精靈中，按兩下 [DNS] 頁面中的表格，輸入新的名稱尾碼。  
  
3.  在 [DNS 伺服器位址] 對話方塊中的 DNS 尾碼，輸入進入點的 ConnectTo 位址，然後按一下 [套用]。  
  
當您新增名稱尾碼但沒有指定伺服器位址時，會將尾碼視為 NRPT 豁免。  
  
## <a name="saving-server-gpo-settings"></a>儲存伺服器 GPO 設定  
**收到的錯誤**。 將遠端存取設定儲存到 GPO < gpo 名稱 > 時發生錯誤。  
  
若要疑難排解這個錯誤，請參閱儲存伺服器 GPO 設定，在[疑難排解啟用多站台](https://technet.microsoft.com/library/jj591658.aspx)。  
  
## <a name="gpo-updates-cannot-be-applied"></a>無法套用 GPO 更新  
**收到的警告**。 < 伺服器名稱 > 上不可套用 GPO 更新。 直到下次重新整理原則之前，變更都不會生效。  
  
**原因**  
  
嘗試重新整理指定電腦上的原則時發生錯誤。 因此，直到下次重新整理原則之前，變更都不會生效。  
  
**解決方法**  
  
若要強制執行原則重新整理，請在指定電腦上執行 `gpupdate /force`。  
  


