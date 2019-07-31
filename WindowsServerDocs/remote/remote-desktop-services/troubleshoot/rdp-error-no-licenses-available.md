---
title: 用戶端無法連線並看到「沒有可用的授權」錯誤
description: 針對遠端桌面連線的沒有可用授權錯誤進行疑難排解
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 540c368812866655452115d8928a915a07cacc97
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/26/2019
ms.locfileid: "68529968"
---
# <a name="clients-cant-connect-and-see-no-licenses-available-error"></a>用戶端無法連線並看到「沒有可用的授權」錯誤

這種情況會發生在包含 RDSH 伺服器和遠端桌面授權伺服器的部署。

首先，找出使用者看到的行為：

- 工作階段已中斷連線，因為沒有可用授權或沒有可用授權伺服器。
- 由於安全性錯誤而拒絕存取。

以網域系統管理員身分登入 RD 工作階段主機，並開啟 RD 授權診斷程式。 尋找如下的訊息：

  - 遠端桌面工作階段主機伺服器的寬限期已過期，但 RD 工作階段主機伺服器尚未透過任何授權伺服器進行設定。 RD 工作階段主機伺服器的連線將遭到拒絕，除非授權伺服器針對 RD 工作階段主機伺服器進行設定。
  - 授權伺服器\<電腦名稱\>無法使用。 這可能是網路連線問題所造成，在授權伺服器上「遠端桌面授權」服務已停止，或 RD 授權無法使用。

這些問題通常與下列使用者訊息相關聯：

  - 遠端工作階段已中斷連線，因為此電腦沒有可用的遠端桌面用戶端存取使用權。
  - 遠端工作階段已中斷連線，因為沒有可用的遠端桌面授權伺服器可提供授權。

在此情況下，[設定 RD 授權服務](#configure-the-rd-licensing-service)。

如果 RD 授權診斷程式列出其他問題，例如「RDP 通訊協定元件 X.224 在通訊協定串流中偵測到錯誤，並已中斷連線用戶端」，則可能存在影響授權憑證的問題。 這類問題通常與如下的使用者訊息相關聯：

由於安全性錯誤，用戶端無法連線到終端機伺服器。 在確定您已登入網路後，再次嘗試連線到伺服器。

在此情況下，[重新整理 X509 憑證登錄機碼](#refresh-the-x509-certificate-registry-keys)。

## <a name="configure-the-rd-licensing-service"></a>設定 RD 授權服務

下列程序會使用伺服器管理員進行設定變更。 有關如何設定及使用伺服器管理員的資訊，請參閱[伺服器管理員](../../../administration/server-manager/server-manager.md)

1. 開啟 [伺服器管理員]  ，然後巡覽至 [遠端桌面服務]  。
2. 在 [部署概觀]  中，選取 [工作]  ，然後選取 [編輯部署內容]  。
3. 選取 [RD 授權]  ，然後為您的部署選取適當的授權模式 (**每一裝置**或**每一使用者**)。
4. 輸入 RD 授權伺服器的完整網域名稱 (FQDN)，然後選取 [新增]  。
5. 如果您有多部 RD 授權伺服器，請為每部伺服器重複步驟 4。 
    ![伺服器管理員中的 RD 授權伺服器設定選項。](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

## <a name="refresh-the-x509-certificate-registry-keys"></a>重新整理 X509 憑證登錄機碼

> [!IMPORTANT]  
> 請仔細遵循本節的指示。 如果您未正確修改登錄，就會發生嚴重問題。 在開始修改登錄之前，請先[備份登錄](https://support.microsoft.com/help/322756)，以便在發生問題時進行還原。

若要解決此問題，請備份並移除 X509 憑證登錄機碼，重新啟動電腦，然後重新啟動 RD 授權伺服器。 依照下列步驟進行。

> [!NOTE]
> 在每個 RDSH 伺服器上執行下列程序。

以下是重新啟用 RD 授權伺服器的方法：

1. 開啟登錄編輯程式，並巡覽至 **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**。
2. 在 [登錄] 功能表上，選取 [匯出登錄檔案]  。
3. 在 [檔案名稱]  方塊中輸入**匯出- 憑證**，然後選取 [儲存]  。
4. 以滑鼠右鍵按一下以下每個值，選取 [刪除]  ，然後選取 [是]  以確認刪除：  
      - **憑證**
      - **X509 憑證**
      - **X509 憑證識別碼**
      - **X509 憑證 2**
5. 結束登錄編輯程式，然後重新啟動 RDSH 伺服器。