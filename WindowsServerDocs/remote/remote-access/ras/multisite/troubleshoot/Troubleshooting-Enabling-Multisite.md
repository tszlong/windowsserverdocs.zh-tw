---
title: 疑難排解啟用多站台
description: 瞭解如何針對與 Enable-DAMultisite 命令相關的問題進行疑難排解。
manager: brianlic
ms.topic: article
ms.assetid: 570c81d6-c4f4-464c-bee9-0acbd4993584
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: abbafdb86edb65efff63eda414baca37e16573fc
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950534"
---
# <a name="troubleshooting-enabling-multisite"></a>疑難排解啟用多站台

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題包含與 `Enable-DAMultisite` 命令問題有關的疑難排解資訊。 若要確認您所收到的錯誤與啟用多站台有關，請檢查 Windows 事件記錄檔中的事件識別碼 10051。

## <a name="user-connectivity-issues"></a>使用者連線問題
如果設定不正確，您啟用多站台時，使用者可能會發生連線問題。

**原因**

在多網站部署中，Windows 10 和 Windows 8 用戶端電腦能夠在不同進入點之間漫遊。  Windows 7 用戶端電腦必須與多網站部署中的特定進入點相關聯。 如果用戶端電腦不在正確的安全性群組中，可能會接收錯誤的群組原則設定。

**解決方案**

DirectAccess 至少需要一個適用于所有 Windows 10 和 Windows 8 用戶端電腦的安全性群組;針對每個網域的所有 Windows 10 和 Windows 8 的電腦，我們建議使用一個安全性群組。 DirectAccess 也需要適用于 Windows 7 用戶端電腦的每個進入點的安全性群組。 每部用戶端電腦應只包含於一個安全性群組中。 因此，您應該確定 Windows 10 和 Windows 8 用戶端的安全性群組只包含執行 Windows 10 或 Windows 8 的電腦，而且每個 Windows 7 用戶端電腦都屬於相關進入點的單一專用安全性群組，而且沒有任何 Windows 10 或 Windows 8 用戶端屬於 Windows 7 安全性群組。

在 [ **DirectAccess 用戶端安裝程式**] 的 [**選取群組**] 頁面上，設定 Windows 8 的安全性群組。 在 [**啟用多網站部署** 嚮導] 的 [**用戶端支援**] 頁面上，或在 [**新增進入點**] 嚮導的 [**用戶端支援**] 頁面上，設定 Windows 7 安全性群組。

## <a name="kerberos-proxy-authentication"></a>Kerberos Proxy 驗證
**收到的錯誤**。 多網站部署不支援 Kerberos proxy 驗證。 您必須啟用電腦憑證的使用，才能進行 IPsec 使用者驗證。

**原因**

啟用多站台之前，必須先啟用電腦憑證驗證。

**解決方案**

若要啟用電腦憑證驗證：

1.  在 [遠端存取管理] 主控台的詳細資料窗格中，按一下 [步驟 2 遠端存取伺服器] 下的 [編輯]。

2.  在 [遠端存取伺服器安裝] 精靈的 [驗證] 頁面中，選取 [使用電腦憑證] 核取方塊，然後選取在部署中發出憑證的根憑證授權單位或中繼憑證授權單位。

若要使用 Windows PowerShell 啟用電腦憑證驗證，請使用 `Set-DAServer` Cmdlet 並指定 *IPsecRootCertificate* 參數。

## <a name="ip-https-certificates"></a>IP-HTTPS 憑證
**收到的錯誤**。 DirectAccess 伺服器使用自我簽署的 IP-HTTPS 憑證。 請設定 IP-HTTPS 使用來自已知 CA 的已簽署憑證。

**原因**

IP-HTTPS 憑證是自我簽署。 您無法在多站台部署中使用自我簽署憑證。

**解決方案**

若要選取 IP-HTTPS 憑證：

1.  在 [遠端存取管理] 主控台的詳細資料窗格中，按一下 [步驟 2 遠端存取伺服器] 下的 [編輯]。

2.  在 [遠端存取伺服器安裝] 精靈的 [網路介面卡] 頁面中，於 [選取用於驗證 IP-HTTPS 連線的憑證] 下，確認已清除 [使用 DirectAccess 自動建立的自我簽署憑證] 核取方塊，然後按一下 [瀏覽] 以選取信任的 CA 發出的憑證。

## <a name="network-location-server"></a>網路位置伺服器

-   **問題 1**

    **收到的錯誤**。 DirectAccess 設定為針對網路位置伺服器使用自我簽署憑證。 請設定網路位置伺服器以使用來自 CA 的已簽署憑證。

    **原因**

    網路位置伺服器部署於遠端存取伺服器上，而且使用自我簽署憑證。 您無法在多站台部署中使用自我簽署憑證。

    **解決方案**

    若要選取網路位置伺服器憑證：

    1.  在 [遠端存取管理] 主控台的詳細資料窗格中，按一下 [步驟 3 基礎結構伺服器] 下的 [編輯]。

    2.  在 [基礎結構伺服器安裝] 精靈的 [網路位置伺服器] 頁面中，於 [網路位置伺服器部署於遠端存取伺服器] 下，確認已清除 [使用自我簽署憑證] 核取方塊，然後按一下 [瀏覽] 以選取企業 CA 發出的憑證。

-   **問題 2**

    **收到的錯誤**。 若要部署網路負載平衡叢集或多網站部署，請取得網路位置伺服器的憑證，其主體名稱與遠端存取服務器的內部名稱不同。

    **原因**

    用於網路位置伺服器網站的憑證主體名稱與遠端存取伺服器的內部名稱相同。 這會造成名稱解析問題。

    **解決方案**

    取得主體名稱與遠端存取伺服器內部名稱不相同的憑證。

    若要設定網路位置伺服器：

    1.  在 [遠端存取管理] 主控台的詳細資料窗格中，按一下 [步驟 3 基礎結構伺服器] 下的 [編輯]。

    2.  在 [基礎結構伺服器安裝] 精靈的 [網路位置伺服器] 頁面中，於 [網路位置伺服器部署於遠端存取伺服器] 下，按一下 [瀏覽] 以選取您之前取得的憑證。 憑證的主體名稱必須與遠端存取伺服器的內部名稱不同。

## <a name="windows-7-client-computers"></a>Windows 7 用戶端電腦
**收到的警告**。 啟用多網站時，為 DirectAccess 用戶端設定的安全性群組不能包含 Windows 7 電腦。 若要在多站台部署中支援 Windows 7 用戶端電腦，請為每個進入點選取含有用戶端的安全性群組。

**原因**

在現有的 DirectAccess 部署中，已啟用 Windows 7 用戶端支援。

**解決方案**

DirectAccess 至少需要一個適用于所有 Windows 8 用戶端電腦的安全性群組，以及適用于每個進入點的 Windows 7 用戶端電腦的安全性群組。 每部用戶端電腦應只包含於一個安全性群組中。 因此，您應該確定 Windows 8 用戶端的安全性群組只包含執行 Windows 8 的電腦，而且每個 Windows 7 用戶端電腦都屬於相關進入點的單一專用安全性群組，而且沒有任何 Windows 8 用戶端屬於 Windows 7 安全性群組。

## <a name="active-directory-site"></a>Active Directory 站台
**收到的錯誤**。 伺服器 <server_name> 與 Active Directory 網站沒有關聯。

**原因**

DirectAccess 無法判斷 Active Directory 站台。 在 [Active Directory 站台及服務] 主控台中，您可以為網路設定不同的子網路，並將每個子網路與相關的 Active Directory 站台關聯。 如果遠端存取伺服器的 IP 位址不屬於任何子網路，或是 IP 位址所屬的子網路未使用 Active Directory 站台定義，就會發生此錯誤。

**解決方案**

確認這是在遠端存取伺服器上執行 `nltest /dsgetsite` 命令的問題。 如果是這個問題，命令會傳回 ERROR_NO_SITENAME。 若要解決此問題，請在網域控制站上，確認已經有包含內部伺服器 IP 位址的子網路，並且使用 Active Directory 站台來定義子網路。

## <a name="saving-server-gpo-settings"></a><a name="SaveGPOSettings"></a>儲存伺服器 GPO 設定
**收到的錯誤**。 將遠端存取設定儲存至 GPO <GPO_name> 時發生錯誤。

**原因**

由於連線問題或 pol 檔案有共用違規，所以無法儲存伺服器 GPO 的變更，例如，另一位使用者已鎖定檔案。

**解決方案**

確認遠端存取伺服器和網域控制站之間有連線能力。 如果有連線能力，請檢查網域控制站，是否有其他使用者鎖定 registry.pol 檔案，並視需要結束該使用者工作階段來解除鎖定檔案。

## <a name="internal-error-occurred"></a><a name="InternalServerError"></a>發生內部錯誤
**收到的錯誤**。 發生內部錯誤。

**原因**

這可能是因為用戶端 GPO 中的非預期進入點表格設定所造成的。 如果系統管理員使用 DirectAccess 用戶端 Cmdlet 編輯用戶端 GPO 中的進入點表格，就會發生這種情況。

**解決方案**

檢閱所有用戶端 GPO 的進入點表格設定，並修正用戶端 GPO 和 DirectAccess 設定的不同執行個體之間，多站台設定中的任何不一致。 使用 `Get-DaEntryPointTableItem` Cmdlet 搭配用戶端 GPO 的名稱，取得用戶端的進入點表格。 使用 `Get-NetIPHttpsConfiguration` Cmdlet 取得所有進入點的所有 IP-HTTPS 設定檔。

如需進一步資訊，請參閱 [Windows PowerShell 中的 DirectAccess 用戶端 Cmdlet](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj591658(v=ws.11))。

