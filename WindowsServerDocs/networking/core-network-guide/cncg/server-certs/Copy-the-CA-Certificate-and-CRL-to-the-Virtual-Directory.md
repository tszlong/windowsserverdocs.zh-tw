---
title: 將 CA 憑證和 CRL 複製到虛擬目錄
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.date: 07/19/2018
ms.openlocfilehash: 275bec5c950ea20c3a7d5a933648cf7e068164d1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318358"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>將 CA 憑證和 CRL 複製到虛擬目錄

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用此程式，將憑證授權單位單位的憑證撤銷清單和企業根 CA 憑證複製到 Web 服務器上的虛擬目錄，並確保 AD CS 已正確設定。 執行下列命令之前，請確定您已將目錄和伺服器名稱取代為適用于您部署的路徑。  
  
若要執行此程式，您必須是**Domain Admins**的成員。  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>將憑證撤銷清單從 CA1 複製到 WEB1  
  
1.  在 CA1 上，以系統管理員身分執行 Windows PowerShell，然後使用下列命令發佈 CRL：  
  
    - 輸入 `certutil -crl`，然後按 ENTER 鍵。  

    - 若要將 CA1 憑證複製到 Web 服務器上的檔案共用，請輸入 `copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki`，然後按 ENTER。  
    
    - 若要將憑證撤銷清單複製到 Web 服務器上的檔案共用，請輸入 `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`，然後按 ENTER。  
  
2.  若要確認您的 CDP 和 AIA 延伸模組位置已正確設定，請輸入 `pkiview.msc`，然後按 ENTER。 Pkiview 企業 PKI MMC 隨即開啟。  
  
3.  在左窗格中，按一下您的 CA 名稱。<p>例如，如果您的 CA 名稱是 corp-CA1-CA，請按一下 [ **corp-CA1-ca**]。 

4. 在 [結果] 窗格的 [狀態] 欄中，確認下列值顯示 [**確定]** ：

    - **CA 憑證**
    - **AIA 位置 #1**
    - **CDP 位置 #1**   
  
  
> [!TIP]  
> 如果任何專案的 [**狀態**] 不是 **[確定]** ，請執行下列動作：  
> -   開啟 Web 服務器上的共用，確認已成功將憑證和憑證撤銷清單檔案複製到共用。 如果未成功複製到共用，請使用正確的檔案來源和共用目的地來修改您的複製命令，然後再次執行命令。  
> -   確認您已在 [CA 延伸模組] 索引標籤上輸入 CDP 和 AIA 的正確位置。請確定您提供的位置中沒有額外的空格或其他字元。  
> -   請確認您已將 CRL 和 CA 憑證複製到 Web 服務器上的正確位置，而且該位置符合您在 CA 上為 CDP 和 AIA 位置提供的位置。  
> -   請確認您已針對儲存 CA 憑證和 CRL 的虛擬資料夾正確設定許可權。  
  


