---
description: 深入瞭解：驗證 DNS 功能以支援目錄複寫
ms.assetid: 709353b0-b913-4367-8580-44745183e2bc
title: 驗證 DNS 功能以支援目錄複寫
ms.author: daveba
ms.date: 05/31/2017
author: Femila
ms.topic: troubleshooting
ms.openlocfilehash: 30a655932d4b19b50cce536545007080b13fd86d
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949234"
---
# <a name="verify-dns-functionality-to-support-directory-replication"></a>驗證 DNS 功能以支援目錄複寫

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

 若要檢查網域名稱系統 (可能會干擾 Active Directory 複寫的 DNS) 設定，您可以先執行基本測試，以確保您的網域能正常運作 DNS。 執行基本測試之後，您可以測試 DNS 功能的其他層面，包括資源記錄註冊和動態更新。

雖然您可以在任何網域控制站上執行這項基本 DNS 功能測試，但您通常會在您認為可能遇到複寫問題的網域控制站上執行這項測試，例如，在事件檢視器 Directory 服務 DNS 記錄檔中報告事件識別碼1844、1925、2087或2088的網域控制站。



## <a name="running-the-domain-controller-basic-dns-testtitle"></a>執行網域控制站基本 DNS 測試</title>

基本 DNS 測試會檢查 DNS 功能的下列層面：


- 連線 **能力：** 測試會判斷是否已在 DNS 中登錄網域控制站、是否可由 <system>ping</system>命令聯繫，以及是否有輕量型目錄存取協定/遠端程序呼叫 (LDAP/RPC) 連線能力。 如果網域控制站上的連線測試失敗，則不會對該網域控制站執行其他測試。 連線能力測試會在執行任何其他 DNS 測試之前自動執行。
- **基本服務：** 測試會確認下列服務正在執行，且可在測試的網域控制站上使用： DNS 用戶端服務、Net Logon 服務、金鑰發佈中心 (KDC) 服務和 DNS 伺服器服務 (是否已將 DNS 安裝在網域控制站) 上。
- **DNS 用戶端設定：**  此測試會確認 DNS 用戶端電腦的所有網路介面卡上的 DNS 伺服器都可以連線。
- **資源記錄註冊：** 此測試會確認主機 (每個網域控制站的) 資源記錄，都至少註冊在用戶端電腦上所設定的其中一部 DNS 伺服器上。
- **(SOA) 的區域和啟動授權單位：** 如果網域控制站正在執行 DNS 伺服器服務，則測試會確認 Active Directory 網域區域的 Active Directory 網域區域和啟動授權單位 (SOA) 資源記錄。
- **根區域：** 檢查根 (. ) 區域是否存在。

若要完成這些程式，至少需要 Enterprise Admins 的成員資格或同等許可權。

您可以使用下列程式來驗證基本的 DNS 功能。

### <a name="to-verify-basic-dns-functionality"></a>若要驗證基本 DNS 功能：


1. 在您要測試的網域控制站上，或已安裝 Active Directory Domain Services (AD DS) 工具的網域成員電腦上，以系統管理員身分開啟命令提示字元。 若要以系統管理員身分開啟命令提示字元，按一下 [**開始**]。
2. 在 [開始搜尋] 中輸入 Command Prompt，
3. 在 [開始] 功能表的頂端，以滑鼠右鍵按一下 [命令提示字元]，然後按一下 [以系統管理員身分執行]。 如果出現 [使用者帳戶控制] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [繼續]。
4. 在命令提示字元中輸入下列命令，然後按 ENTER 鍵： `dcdiag /test:dns /v /s:<DCName> /DnsBasic /f:dcdiagreport.txt`
</br></br>以 DCName 的網域控制站實際的辨別名稱、NetBIOS 名稱或 DNS 名稱替代 &lt; &gt; 。 或者，您可以輸入/e：而非/s：來測試樹系中的所有網域控制站。
/F 參數會指定檔案名，在上一個命令中 dcdiagreport.txt。 如果您想要將檔案放在目前工作目錄以外的位置，則可以指定檔案路徑，例如/f:c:reportsdcdiagreport.txt。

5. 在 [記事本] 或類似的文字編輯器中開啟 dcdiagreport.txt 檔案。 若要在 [記事本] 中開啟檔案，請在命令提示字元中輸入 Notepad dcdiagreport.txt，然後按 ENTER 鍵。 如果您將檔案放在不同的工作目錄中，請包含檔案的路徑。 例如，如果您將檔案放在 c:reports 中，請輸入 notepad c:reportsdcdiagreport.txt，然後按 ENTER 鍵。
6. 滾動至接近檔案底部的摘要資料表。
</br></br>請注意摘要表中報告「警告」或「失敗」狀態的所有網域控制站的名稱。  藉由搜尋字串 "DC： DCName" （其中 DCName 是網域控制站的實際名稱）來尋找詳細的分組區段，以判斷是否有問題的網域控制站。

如果您看到需要的明確設定變更，請視需要進行。 例如，如果您注意到其中一個網域控制站的 IP 位址很明顯不正確，您可以更正它。 然後，重新執行測試。

若要驗證設定變更，請視需要使用/e：或/s： switch 重新執行 Dcdiag/test： DNS/v 命令。 如果您沒有在網域控制站上啟用 IP 第6版 (IPv6) ，您應該預期主機 (AAAA) 驗證部分會失敗，但如果您的網路上未使用 IPv6，則不需要這些記錄。

## <a name="verifying-resource-record-registration"></a>正在驗證資源記錄註冊

目的地網域控制站會使用 DNS 別名 (CNAME) 資源記錄，以找出其來源網域控制站複寫夥伴。 雖然執行 Windows Server 的網域控制站 (從 Windows Server 2003 （含 Service Pack 1） (SP1) # A3）可以使用完整功能變數名稱 (Fqdn 尋找來源複寫協力電腦) 或者，如果失敗，則會預期別名 (CNAME) 資源記錄的 NetBIOS namesthe 存在，而且應該驗證是否有適當的 DNS 運作。

您可以使用下列程式來確認資源記錄註冊，包括別名 (CNAME) 資源記錄註冊。

### <a name="to-verify-resource-record-registrationtitle"></a>確認資源記錄註冊</title>


1. 以系統管理員身分開啟命令提示字元。 若要以系統管理員身分開啟命令提示字元，按一下 [開始]。 在 [開始搜尋] 中輸入 Command Prompt，
2. 在 [開始] 功能表的頂端，以滑鼠右鍵按一下 [命令提示字元]，然後按一下 [以系統管理員身分執行]。 如果出現 [使用者帳戶控制] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [繼續]。  </br></br>您可以使用 Dcdiag 工具，藉由執行命令來確認註冊網域控制站位置的所有必要資源記錄 `dcdiag /test:dns /DnsRecordRegistration` 。

此命令會驗證 DNS 中的下列資源記錄註冊：


- **別名 (CNAME) ：** 找出複寫夥伴的全域唯一識別碼 (GUID) 架構資源記錄
- **主機 () ：**  包含網域控制站 IP 位址的主機資源記錄
- **LDAP SRV：** 服務 (SRV) 找出 LDAP 伺服器的資源記錄
- **GC SRV**：服務 (SRV) 找出通用類別目錄伺服器的資源記錄
- **PDC SRV**：服務 (SRV) 資源記錄，以找出網域主控站 (PDC) 模擬器操作主機

您可以使用下列程式單獨驗證別名 (CNAME) 資源記錄註冊。

### <a name="to-verify-alias-cname-resource-record-registration"></a>確認別名 (CNAME) 資源記錄註冊

1. 開啟 DNS 嵌入式管理單元。 若要開啟 DNS，請按一下 [開始]。 在 [開始搜尋] 中，輸入 dnsmgmt.msc，然後按 ENTER。 如果出現 [使用者帳戶控制] 對話方塊，請確認它顯示您想要的動作，然後按一下 [繼續]。
2. 您可以使用 DNS 嵌入式管理單元來尋找任何執行 DNS 伺服器服務的網域控制站，其中伺服器會使用與網域控制站的 Active Directory 網域相同的名稱來裝載 DNS 區域。
3. 在主控台樹中，按一下名為 _msdcs 的區域。Dns_Domain_Name。
4. 在詳細資料窗格中，確認有下列資源記錄存在：別名 (CNAME) 名為 Dsa_Guid. _msdcs 的資源記錄。<placeholder>Dns_Domain_Name</placeholder> 和對應的主機 (Dns 伺服器名稱的) 資源記錄。

如果別名 (CNAME) 資源記錄未註冊，請確認動態更新是否正常運作。 使用下一節中的測試來驗證動態更新。

## <a name="verifying-dynamic-update"></a>驗證動態更新

如果基本 DNS 測試顯示 DNS 中沒有資源記錄，請使用「動態更新測試」來判斷 Net Logon 服務未自動註冊資源記錄的原因。 若要確認 Active Directory 網域區域已設定為接受安全動態更新，並 (_dcdiag_test_record) 中執行測試記錄的註冊，請使用下列程式。 測試記錄會在測試之後自動刪除。

### <a name="to-verify-dynamic-updatetitle"></a>驗證動態更新</title>


1. 以系統管理員身分開啟命令提示字元。 若要以系統管理員身分開啟命令提示字元，按一下 [開始]。 在 [開始搜尋] 中輸入 Command Prompt， 在 [開始] 功能表的頂端，以滑鼠右鍵按一下 [命令提示字元]，然後按一下 [以系統管理員身分執行]。 如果出現 [使用者帳戶控制] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [繼續]。
2. 在命令提示字元中輸入下列命令，然後按 ENTER 鍵： `dcdiag /test:dns /v /s:<DCName> /DnsDynamicUpdate`
   </br></br>以 DCName 的網域控制站的辨別名稱、NetBIOS 名稱或 DNS 名稱替代 &lt; &gt; 。 或者，您可以輸入/e：而非/s：來測試樹系中的所有網域控制站。 如果您未在網域控制站上啟用 IPv6，您應該預期主機 (AAAA) 資源記錄部分的測試將會失敗，這是未啟用 IPv6 時的正常情況。

如果未設定安全動態更新，您可以使用下列程式來設定它們。

### <a name="to-enable-secure-dynamic-updates"></a>啟用安全動態更新


1. 開啟 DNS 嵌入式管理單元。 若要開啟 DNS，請按一下 [開始]。
2. 在 [開始搜尋] 中，輸入 dnsmgmt.msc，然後按 ENTER。 如果出現 [使用者帳戶控制] 對話方塊，請確認它顯示您想要的動作，然後按一下 [繼續]。
3. 在主控台樹狀目錄的適用區域上按一下滑鼠右鍵，然後按一下 [內容]。
4. 在 [一般] 索引標籤上，確認區域類型是否為 [Active Directory 整合]。
5. 在 [動態更新] 中，按一下 [僅安全]。

## <a name="registering-dns-resource-records"></a>註冊 DNS 資源記錄

如果 DNS 資源記錄未出現在來源網域控制站的 DNS 中，您已驗證動態更新，而且您想要立即註冊 DNS 資源記錄，您可以使用下列程式手動強制註冊。 網域控制站上的 Net Logon 服務會註冊要在網路上放置網域控制站所需的 DNS 資源記錄。 DNS 用戶端服務會將主機 () 的資源記錄， (CNAME) 記錄指向該別名。

### <a name="to-register-dns-resource-records-manuallytitle"></a>手動註冊 DNS 資源記錄</title>


1. 以系統管理員身分開啟命令提示字元。 若要以系統管理員身分開啟命令提示字元，按一下 [開始]。
2. 在 [開始搜尋] 中輸入 Command Prompt，
3. 在 [開始] 頂端，以滑鼠右鍵按一下 [命令提示字元]，然後按一下 [以系統管理員身分執行]。 如果出現 [使用者帳戶控制] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [繼續]。
4. 若要在來源網域控制站上手動起始網域控制站定位器資源記錄的註冊，請在命令提示字元中輸入下列命令，然後按 ENTER 鍵： `net stop netlogon && net start netlogon`
5. 若要手動起始) 資源記錄的主機註冊 (，請在命令提示字元中輸入下列命令，然後按 ENTER 鍵： `ipconfig /flushdns && ipconfig /registerdns`
6. 在命令提示字元中輸入下列命令，然後按 ENTER 鍵： `dcdiag /test:dns /v /s:<DCName>` </br></br>以 DCName 的網域控制站的辨別名稱、NetBIOS 名稱或 DNS 名稱替代 &lt; &gt; 。 檢查測試的輸出，以確保已通過 DNS 測試。 如果您未在網域控制站上啟用 IPv6，您應該預期主機 (AAAA) 資源記錄部分的測試將會失敗，這是未啟用 IPv6 時的正常情況。
