---
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: 驗證 DNS 功能以支援目錄複寫
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: identity-adds
ms.author: joflore
ms.date: 05/31/2017
ms.tgt_pltfrm: na
author: Femila
ms.openlocfilehash: a55b95ee516abda8bdbae6e9829a161ef060012e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871869"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>驗證 DNS 功能以支援目錄複寫

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

 若要檢查可能會干擾 Active Directory 複寫的網域名稱系統 (DNS) 設定，您可以開始執行基本測試，以確保您的網域 DNS 正常運作。 執行基本測試之後，您可以測試 DNS 的功能，包括資源記錄登錄及動態更新的其他層面。

雖然您可以執行這項測試的任何網域控制站，通常在您的想法的網域控制站上執行這項測試的基本 DNS 功能，可能就會發生複寫問題，比方說，可報告事件識別碼 1844 1925、 2087、 網域控制站或在 事件檢視器目錄服務的 DNS 記錄 2088。



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>執行網域控制站基本 DNS 測試</title>

基本的 DNS 測試會檢查 DNS 功能的下列層面：


- **連線能力：** 測試會判斷所是否可以連線到的網域控制站會在 DNS 中登錄<system>ping</system>命令，並有輕量型目錄存取通訊協定 / 遠端程序呼叫 (LDAP/RPC) 連線。 如果連線測試失敗的網域控制站上，針對該網域控制站不執行任何其他測試。 任何其他 DNS 測試執行之前，會自動執行連線測試。
- **基本的服務：** 測試可確認下列服務已執行及測試的網域控制站上使用：DNS 用戶端服務、 Net Logon 服務、 金鑰發佈中心 (KDC) 服務和 DNS 伺服器服務 （如果有 DNS 已安裝網域控制站上）。
- **DNS 用戶端設定：** 測試會驗證所有網路介面卡的 DNS 用戶端電腦上的 DNS 伺服器可連線。
- **資源記錄登錄：** 測試可確認每個網域控制站的主機 (A) 資源記錄在至少一個用戶端電腦設定的 DNS 伺服器上註冊。
- **區域和啟動授權 (SOA):** 如果網域控制站正在執行的 DNS 伺服器服務，測試會確認都存在的 Active Directory 網域的區域與 Active Directory 網域區域的授權 (SOA) 資源記錄的開始時間。
- **根區域：** 檢查是否有根 （.） 區域。

要完成這些程序，至少需要的成員資格 Enterprise Admins 或同等權限。

若要確認 DNS 的基本功能，您可以使用下列程序。
     
### <a name="to-verify-basic-dns-functionality"></a>若要確認 DNS 的基本功能：


1. 在您想要測試的網域控制站上，或在已安裝的 Active Directory 網域服務 (AD DS) 工具的網域成員電腦上，請系統管理員身分開啟命令提示字元。 若要以系統管理員身分開啟命令提示字元，按一下 [**開始**]。 
2. 在 開始搜尋 中輸入 命令提示字元。 
3. 在 開始 功能表的頂端，以滑鼠右鍵按一下 命令提示字元中，然後按一下 以系統管理員身分執行 如果出現 [使用者帳戶控制] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [繼續]。
4. 在命令提示字元中，輸入下列命令，並按 ENTER: `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>取代實際的辨別的名稱、 NetBIOS 名稱或 DNS 名稱的網域控制站&lt;DCName&gt;。 或者，您可以輸入而不是 /s: /e 樹系中測試所有網域控制站。 /F 參數會指定檔案名稱，這在前一個命令是 dcdiagreport.txt。 如果您想要將檔案放在目前的工作目錄以外的位置中，您可以指定檔案路徑，例如 /f:c:reportsdcdiagreport.txt。

5. 在 [記事本] 或類似的文字編輯器中開啟 dcdiagreport.txt 檔案。 在記事本中，開啟檔案，在命令提示字元中，輸入 notepad dcdiagreport.txt，然後按 ENTER。 如果您將檔案放在不同的工作目錄中，包含檔案的路徑。 比方說，如果您將檔案置於 c:reports 時，輸入 notepad c:reportsdcdiagreport.txt，然後按 ENTER 鍵。
6. 捲動到檔案底部附近的 [摘要] 表格。 
</br></br>請注意該報表的摘要資料表中的 「 警告 」 或 「 失敗 」 狀態的所有網域控制站的名稱。  嘗試判斷搜尋字串，找出詳細的 breakout 區段是否有問題的網域控制站 「 DC:DCName，「 DCName 所在的網域控制站的實際名稱。

如果您看到所需的明顯的組態變更，因此，視需要。 例如，如果您注意到其中一個網域控制站有很明顯地不正確的 IP 位址，您可以修正它。 然後，重新執行測試。

若要驗證的組態變更，請重新執行與 /e 或 /s： 參數，適當地 Dcdiag /test /v 命令。 如果您沒有 IP 第 6 版 (IPv6 的網域控制站上啟用)，您應該預期的主機 (AAAA) 驗證部分的測試失敗，但如果您未在網路上使用 IPv6，這些記錄不一定。
            
## <a name="verifying-resource-record-registration"></a>正在驗證資源記錄登錄
    
目的地網域控制站會使用 DNS 別名 (CNAME) 資源記錄，以找出與其來源網域控制站複寫協力電腦。 雖然執行 Windows Server （從 Windows Server 2003 含 Service Pack 1 (SP1) 開始） 的網域控制站就可以找到來源複寫協力電腦使用完整的網域名稱 (Fqdn) 或，如果失敗，NetBIOS namesthe 存在的別名 (CNAME)資源記錄會預期，而且應該驗證適當運作的 dns。 
      
若要確認資源記錄登錄，包括別名 (CNAME) 資源記錄登錄，您可以使用下列程序。
      
### <a name="to-verify-resource-record-registrationtitle"></a>若要確認資源記錄登錄</title>


1. 以系統管理員身分開啟命令提示字元。 若要開啟命令提示字元，以系統管理員，按一下 啟動。 在 開始搜尋 中輸入 命令提示字元。 
2. 在 開始 功能表的頂端，以滑鼠右鍵按一下 命令提示字元中，然後按一下 以系統管理員身分執行 如果出現 [使用者帳戶控制] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [繼續]。  </br></br>您可以使用 Dcdiag 工具來驗證所不可或缺的網域控制站位置執行的所有資源記錄的登錄`dcdiag /test:dns /DnsRecordRegistration`命令。

此命令會驗證下列資源記錄在 DNS 中的登錄：


- **別名 (CNAME):** 的全域唯一識別碼 (GUID)-以找出複寫協力電腦的資源記錄
- **主機 (A):** 包含網域控制站的 IP 位址的主機資源記錄
- **LDAP SRV:** 找出 LDAP 伺服器的服務 (SRV) 資源記錄
- **GC 的 SRV**： 服務 (SRV) 資源記錄，找出通用類別目錄伺服器
- **PDC SRV**： 找出主要網域控制站 (PDC) 模擬器操作主機的服務 (SRV) 資源記錄

若要確認別名 (CNAME) 資源記錄登錄本身，您可以使用下列程序。

### <a name="to-verify-alias-cname-resource-record-registration"></a>若要確認別名 (CNAME) 資源記錄登錄

1. 開啟 [DNS] 嵌入式管理單元。 若要開啟 DNS，按一下 啟動。 在 [開始搜尋] 中輸入 dnsmgmt.msc，然後再按 ENTER 鍵。 如果出現 [使用者帳戶控制] 對話方塊中，確認它會顯示您想要，然後按一下 [繼續] 的動作。
2. 找出任何正在執行 DNS 伺服器服務，其中伺服器裝載 DNS 區域與 Active Directory 網域的網域控制站的名稱相同的網域控制站，請使用 [DNS] 嵌入式管理單元。
3. 在主控台樹狀目錄中，按一下名為 _msdcs 區域。Dns_Domain_Name。
4. 在 詳細資料 窗格中，確認 下列資源記錄會出現： 名為 Dsa_Guid._msdcs 別名 (CNAME) 資源記錄。<placeholder>Dns_Domain_Name</placeholder>以及對應的主機 (A) 資源記錄，DNS 伺服器的名稱。

如果未註冊的別名 (CNAME) 資源記錄，確認已正確運作，動態更新。 使用下一節中的測試，確認動態更新。
    
## <a name="verifying-dynamic-update"></a>正在驗證動態更新
    
如果基本 DNS 測試會顯示在 DNS 中不存在的資源記錄，請判斷為什麼 Net Logon 服務並未註冊的資源記錄會自動使用動態更新測試。 若要確認已設定 Active Directory 網域的區域，以接受安全動態更新，並執行的測試記錄 (_dcdiag_test_record) 的註冊，請使用下列程序。 測試完成之後自動刪除該測試記錄。

### <a name="to-verify-dynamic-updatetitle"></a>若要確認動態更新</title>


1. 以系統管理員身分開啟命令提示字元。 若要開啟命令提示字元，以系統管理員，按一下 啟動。 在 開始搜尋 中輸入 命令提示字元。 在 開始 功能表的頂端，以滑鼠右鍵按一下 命令提示字元中，然後按一下 以系統管理員身分執行 如果出現 [使用者帳戶控制] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [繼續]。
2. 在命令提示字元中，輸入下列命令，並按 ENTER: `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>取代的辨別的名稱、 NetBIOS 名稱或 DNS 名稱的網域控制站&lt;DCName&gt;。 或者，您可以輸入而不是 /s: /e 樹系中測試所有網域控制站。 如果您還沒有網域控制站上啟用 IPv6，您應該預期的主機 (AAAA) 資源記錄部分測試失敗，也就是未啟用 IPv6 時的正常情況。

如果未設定安全動態更新，您可以使用下列程序來設定它們。

### <a name="to-enable-secure-dynamic-updates"></a>若要啟用安全動態更新


1. 開啟 [DNS] 嵌入式管理單元。 若要開啟 DNS，按一下 啟動。 
2. 在 [開始搜尋] 中輸入 dnsmgmt.msc，然後再按 ENTER 鍵。 如果出現 [使用者帳戶控制] 對話方塊中，確認它會顯示您想要然後按一下 [繼續] 的動作。
3. 在主控台樹狀目錄中，以滑鼠右鍵按一下適用的區域，然後按一下屬性。
4. 在 一般 索引標籤中，確認區域類型是 Active Directory 整合。
5. 在動態更新，請只按一下安全。

## <a name="registering-dns-resource-records"></a>註冊 DNS 資源記錄
    
如果來源網域控制站的 DNS 資源記錄不會出現在 DNS 中，您已動態更新，和您想要立即註冊 DNS 資源記錄，您可以使用下列程序，手動強制註冊。 Net Logon 服務在網域控制站上的註冊所需的網域控制站位於網路上的 DNS 資源記錄。 DNS 用戶端服務會註冊別名 (CNAME) 記錄指向主機 (A) 資源記錄。

### <a name="to-register-dns-resource-records-manuallytitle"></a>若要以手動方式註冊的 DNS 資源記錄</title>


1. 以系統管理員身分開啟命令提示字元。 若要開啟命令提示字元，以系統管理員，按一下 啟動。 
2. 在 開始搜尋 中輸入 命令提示字元。 
3. 在頂端開始，以滑鼠右鍵按一下 命令提示字元中，然後按一下 以系統管理員身分執行 如果出現 [使用者帳戶控制] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [繼續]。
4. 若要起始註冊的網域控制站定位程式資源記錄，以手動方式在來源網域控制站，在命令提示字元中，輸入下列命令，，，然後按 ENTER: `net stop netlogon && net start netlogon`
5. 若要起始註冊的主機 (A) 資源記錄以手動的方式，在命令提示字元中，輸入下列命令，然後按 ENTER 鍵： `ipconfig /flushdns && ipconfig /registerdns`
6. 在命令提示字元中，輸入下列命令，並按 ENTER: `dcdiag /test:dns /v /s:<DCName>` </br></br>取代的辨別的名稱、 NetBIOS 名稱或 DNS 名稱的網域控制站&lt;DCName&gt;。 檢閱輸出，以確保 DNS 測試成功的測試。 如果您還沒有網域控制站上啟用 IPv6，您應該預期的主機 (AAAA) 資源記錄部分測試失敗，也就是未啟用 IPv6 時的正常情況。
