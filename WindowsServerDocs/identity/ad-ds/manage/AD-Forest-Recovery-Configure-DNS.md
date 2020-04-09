---
title: AD 樹系復原-設定 DNS 伺服器服務
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 144a45f2a835d9cca60b5be5aac7569809c45b7c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824171"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>AD 樹系復原-設定 DNS 伺服器服務

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

如果您從備份還原的 DC 上未安裝 DNS 伺服器角色，您必須安裝和設定 DNS 伺服器。 

## <a name="install-and-configure-the-dns-server-service"></a>安裝和設定 DNS 伺服器服務

在還原完成之後，針對未在 DNS 伺服器上執行的每個已還原 DC 完成此步驟。 

> [!NOTE]
> 如果您從備份還原的 DC 正在執行 Windows Server 2008 R2，您必須將 DC 連線到隔離的網路，才能安裝 DNS 伺服器。 然後將每個已還原的 DNS 伺服器連接到相互共用的隔離網路。 執行 repadmin/replsum，確認還原的 DNS 伺服器之間的複寫正常運作。 在確認複寫之後，如果已安裝 DNS 伺服器角色，您可以將還原的 Dc 連接到生產網路，您可以套用可讓 DNS 伺服器在伺服器未連線到任何網路時啟動的修補程式。 在您的自動化組建程式期間，您應該將此修補程式彙集到作業系統安裝映射中。 如需有關此修補程式的詳細資訊，請參閱 Microsoft 知識庫中的[文章 975654](https://go.microsoft.com/fwlink/?LinkId=184691) （ https://go.microsoft.com/fwlink/?LinkId=184691)。 

完成以下的安裝和設定步驟。

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>若要使用伺服器管理員安裝和 DNS 伺服器服務  

1. 開啟伺服器管理員，然後按一下 [**新增角色及功能**]。 
2. 在 [新增角色精靈] 中，如果出現 [**在您開始前**] 頁面，請按 [**下一步**]。 
3. 在 [**安裝類型**] 畫面上，選取 [**角色型或功能型安裝**]，然後按 **[下一步]** 。
4. 在 [**伺服器選取專案**] 畫面上選取伺服器，然後按 **[下一步]** 。
5. 在 **伺服器角色** 畫面上選取  **DNS 伺服器**，如果出現提示，請按一下 **新增功能**，然後按一下
6. 在 [**功能**] 畫面上，按 **[下一步]** 。
7. 閱讀 [ **DNS 伺服器**] 頁面上的資訊，然後按 **[下一步]** 。
   ![DNS 伺服器](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8. 在 [**確認**] 頁面上，確認將會安裝 DNS 伺服器角色，然後按一下 [**安裝**]。 

### <a name="to-configure-the-dns-server-service"></a>設定 DNS 伺服器服務

1. 開啟伺服器管理員，按一下 [**工具**]，然後按一下 [ **DNS**]。
   ![DNS 伺服器](media/AD-Forest-Recovery-Configure-DNS/dns2.png)
2. 為 DNS 伺服器上裝載的相同 DNS 功能變數名稱建立 DNS 區域，然後才會發生嚴重的故障。 如需詳細資訊，請參閱新增正向對應區域（[https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574)）。
3. 在嚴重的故障之前，設定 DNS 資料。 例如，  

   - 設定要儲存在 AD DS 中的 DNS 區域。 如需詳細資訊，請參閱變更區欄位型別（[https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579)）。
   - 設定網域控制站定位程式（DC 定位器）資源記錄的授權 DNS 區域，以允許安全的動態更新。 如需詳細資訊，請參閱只允許安全的動態更新（[https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580)）。

4. 請確定父 DNS 區域包含此 DNS 伺服器上主控之子領域的委派資源記錄（名稱伺服器（NS）和粘附主機（A）資源記錄）。 如需詳細資訊，請參閱建立區域委派（[https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562)）。
5. 設定 DNS 之後，您可以加速註冊 NETLOGON 記錄。

   > [!NOTE]
   > 只有在有可用的通用類別目錄伺服器時，才能使用安全動態更新。 

   在命令提示字元，輸入下列命令，然後按 ENTER：  

   **net stop netlogon**  

6. 輸入下列命令，然後按下 ENTER：  

   **net start netlogon**  

   ![DNS 伺服器](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
