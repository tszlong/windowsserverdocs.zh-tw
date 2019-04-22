---
title: 疑難排解啟用 OTP
description: 本主題是 Windows Server 2016 中的 OTP 驗證部署遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d789671a0425974e560e5f4a43ebcba4c1741c76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819459"
---
# <a name="troubleshooting-enabling-otp"></a>疑難排解啟用 OTP

>適用於：Windows Server （半年通道），Windows Server 2016

本主題包含啟用 DirectAccess OTP 驗證使用的相關問題的疑難排解資訊**啟用 DAOtpAuthentication** PowerShell cmdlet 或 遠端存取管理主控台。
  
## <a name="failed-to-enroll-the-otp-signing-certificate"></a>無法註冊 OTP 簽章憑證  
**收到的錯誤**（伺服器事件記錄檔）。 無法使用憑證範本 < OTP_signing_template_name > 註冊 OTP 簽章憑證  
  
**原因**  
  
有三個可能的原因造成此錯誤：  
  
-   範本不存在。  
  
-   在範本上設定的權限不允許 DirectAccess 伺服器註冊。  
  
-   沒有網路連線到發行的憑證授權單位 (CA)。  
  
**解決方法**  
  
1.  請確定 OTP 簽署的憑證範本，以指定的名稱：  
  
    1.  而且有適當的權限。  
  
    2.  設定為至少一個可以發出憑證給 DirectAccess 伺服器的 CA 所發出。  
  
2.  如果範本不存在、 建立中所述 3.3 的計劃的登錄授權單位憑證，，或如果另一個相符的範本重新 DirectAccess OTP 設定新的範本名稱。  
  
## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>無法安裝 WebDAV 時啟用 DirectAccess OTP  
**案例**。 嘗試套用 DirectAccess OTP 設定遠端存取管理主控台中，或使用`Enable-DAOtpAuthentication`PowerShell cmdlet，則作業會失敗。  
  
**收到的錯誤**（伺服器事件記錄檔）。 DirectAccess OTP 設定無法套用，因為 WebDAV IIS 擴充功能在伺服器上執行。 移除 WebDAV，然後重新套用設定。  
  
**原因**  
  
DirectAccess OTP 服務與 WebDAV 發行的功能不相容，而且無法在安裝 WebDAV 時啟用。  
  
**解決方法**  
  
解除安裝 WebDAV 角色：  
  
1.  在 [伺服器管理員] 主控台中，在左窗格中，按一下**IIS**。  
  
2.  在主窗格中，捲動到**角色和功能**。  
  
3.  以滑鼠右鍵按一下**WebDAV 發行**，然後按一下**移除角色或功能**。  
  
4.  完成 [移除角色及功能精靈]。  
  
5.  重新套用 DirectAccess OTP 設定。  
  
## <a name="no-templates-available-in-the-remote-access-management-console"></a>沒有在遠端存取管理主控台中可用的範本  
**案例**。 雖然從選取範圍 視窗中設定一些，使用 遠端存取管理 主控台的 OTP 或註冊授權單位憑證範本，或所有的範本已遺失。  
  
**原因**  
  
有兩個可能的原因造成此錯誤：  
  
-   範本未設定根據 DirectAccess OTP 需求，因此無法選取。  
  
-   在選取的 Ca **OTP CA 伺服器**未設定為發行所需的範本。  
  
**解決方法**  
  
1.  請確定 3.2 OTP 憑證範本和 3.3 計劃的登錄授權單位憑證的計劃中所述正確設定 OTP 登入的範本和 OTP 簽署的憑證範本。  
  
2.  請確定中設定的 Ca **OTP CA 伺服器**清單會設定為問題相關的範本：  
  
    1.  在 CA 伺服器上，開啟 憑證授權單位主控台。  
  
    2.  在左窗格中，展開 選擇的 CA 伺服器。  
  
    3.  按一下 **憑證範本**，並確定已啟用必要的範本。 如果沒有，請以滑鼠右鍵按一下**憑證範本**，按一下**新增**，按一下 **要發出的憑證範本**，然後選取您想要啟用的範本。  
  
## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>無法將的 OTP 範本的更新間隔設定為 1 小時  
**案例**。 在設定使用 Windows 2003 CA 的 DirectAccess OTP 登入範本時，它不可能將範本的更新間隔設定為 1 小時。  
  
**原因**  
  
在 Windows Server 2003 憑證範本 MMC 嵌入式管理單元不允許您將範本的更新間隔設定為 1 小時。  
  
**解決方法**  
  
後續的 Windows Server 2003 伺服器上安裝憑證範本 嵌入式管理單元，並用來設定 OTP 登入範本，請參閱 <<c0> [ 憑證範本 嵌入式管理單元安裝](https://technet.microsoft.com/library/cc732445.aspx)。  
  


