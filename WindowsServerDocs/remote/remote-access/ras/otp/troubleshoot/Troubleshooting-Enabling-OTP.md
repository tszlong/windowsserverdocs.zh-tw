---
title: 疑難排解啟用 OTP
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署遠端存取指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9be7ef4c4d07b522f683a403e46a11e109dbd226
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853641"
---
# <a name="troubleshooting-enabling-otp"></a>疑難排解啟用 OTP

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題包含有關使用**DAOtpAuthentication** PowerShell Cmdlet 或遠端存取管理主控台啟用 DirectAccess OTP 驗證之問題的疑難排解資訊。
  
## <a name="failed-to-enroll-the-otp-signing-certificate"></a>無法註冊 OTP 簽署憑證  
**收到的錯誤**（伺服器事件記錄檔）。 無法使用憑證範本 < OTP_signing_template_name 註冊 OTP 簽署憑證 >  
  
**原因**  
  
此錯誤有三個可能的原因：  
  
-   範本不存在。  
  
-   在範本上設定的許可權不允許 DirectAccess 伺服器進行註冊。  
  
-   沒有與頒發證書授權單位（CA）的網路連線。  
  
**解決方法**  
  
1.  請確定具有指定名稱的 OTP 簽署憑證範本：  
  
    1.  存在且具有適當的許可權。  
  
    2.  設定為由至少一個可頒發證書給 DirectAccess 伺服器的 CA 發行。  
  
2.  如果範本不存在，請依照3.3 規劃登錄授權單位憑證中所述的方式加以建立，或者如果另一個相符的範本存在，請使用新的範本名稱重新設定 DirectAccess OTP。  
  
## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>安裝 WebDAV 時無法啟用 DirectAccess OTP  
**案例**。 嘗試在 [遠端存取管理] 主控台中或藉由使用 `Enable-DAOtpAuthentication` PowerShell Cmdlet 來套用 DirectAccess OTP 設定時，作業會失敗。  
  
**收到的錯誤**（伺服器事件記錄檔）。 因為 WebDAV IIS 擴充功能正在伺服器上執行，所以無法套用 DirectAccess OTP 設定。 請移除 WebDAV，然後再次套用設定。  
  
**原因**  
  
DirectAccess OTP 服務與 WebDAV 發行功能不相容，而且在安裝 WebDAV 時無法啟用。  
  
**解決方法**  
  
卸載 WebDAV 角色：  
  
1.  在伺服器管理員主控台中，按一下左窗格中的 [ **IIS**]。  
  
2.  在主窗格中，流覽至 [**角色及功能**]。  
  
3.  以滑鼠右鍵按一下 [ **WebDAV 發行**]，然後按一下 [**移除角色或功能**]。  
  
4.  完成 [移除角色及功能]。  
  
5.  重新套用 DirectAccess OTP 設定。  
  
## <a name="no-templates-available-in-the-remote-access-management-console"></a>遠端存取管理主控台中沒有可用的範本  
**案例**。 使用遠端存取管理主控台來設定 OTP 或登錄授權單位憑證範本時，選取範圍視窗中缺少部分或所有範本。  
  
**原因**  
  
此錯誤有兩個可能的原因：  
  
-   範本未根據 DirectAccess OTP 需求進行設定，因此無法選取。  
  
-   在**OTP CA 伺服器**底下選取的 ca 未設定為發出所需的範本。  
  
**解決方法**  
  
1.  請確定 OTP 登入範本和 OTP 簽署憑證範本已正確設定，如3.2 規劃 OTP 憑證範本和3.3 規劃登錄授權單位憑證中所述。  
  
2.  請確定已設定**OTP Ca 伺服器**清單中設定的 ca，以發出相關的範本：  
  
    1.  在 CA 伺服器上，開啟 [憑證授權單位單位] 主控台。  
  
    2.  在左窗格中，展開所選的 CA 伺服器。  
  
    3.  按一下 [**憑證範本**]，並確定已啟用必要的範本。 如果沒有，請以滑鼠右鍵按一下 [**憑證範本**]，按一下 [**新增**]，按一下 [**要發出的憑證範本**]，然後選取您要啟用的範本。  
  
## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>無法將 OTP 範本的更新期間設定為1小時  
**案例**。 使用 Windows 2003 CA 設定 DirectAccess OTP 登入範本時，不可能將範本的更新期間設為1小時。  
  
**原因**  
  
Windows Server 2003 中的 [憑證範本] MMC 嵌入式管理單元不允許您將範本的更新期間設定為1小時。  
  
**解決方法**  
  
在 Windows Server 2003 伺服器後安裝憑證範本嵌入式管理單元，並使用它來設定 OTP 登入範本，請參閱[安裝憑證範本嵌入式管理](https://technet.microsoft.com/library/cc732445.aspx)單元。  
  


