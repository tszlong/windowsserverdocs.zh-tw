---
title: 將 CA 憑證與 CRL 複製到虛擬目錄
description: 本主題是本指南適用於 802.1x 有線和無線部署的部署伺服器憑證的一部分
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/19/2018
ms.openlocfilehash: 15cc807db805e1be0349ea51119663e515c7e551
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874029"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>將 CA 憑證與 CRL 複製到虛擬目錄

>適用於：Windows Server （半年通道），Windows Server 2016

若要將從您的憑證授權單位的憑證撤銷清單，以及企業根 CA 憑證複製到您的 Web 伺服器上的虛擬目錄，並確保已正確設定 AD CS，您可以使用此程序。 之前執行下列命令，請確定您的同等適用於您的部署取代目錄和伺服器名稱。  
  
若要執行此程序中，您必須是隸屬**Domain Admins**。  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>若要從 CA1 的憑證撤銷清單複製至 WEB1  
  
1.  在 CA1，以系統管理員身分執行 Windows PowerShell，然後將發行的 CRL，使用下列命令：  
  
    - 輸入 `certutil -crl`，然後按 ENTER 鍵。  

    - 若要將憑證撤銷清單複製到您的 Web 伺服器上的檔案共用中，輸入`copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`，然後按 ENTER 鍵。  
  
2.  若要確認已正確設定您的 CDP 和 AIA 延伸模組位置，請輸入`pkiview.msc`，然後按 ENTER 鍵。 Pkiview 企業 PKI MMC 隨即開啟。  
  
3.  在左窗格中，按一下 CA 名稱。<p>例如，如果您的 CA 名稱是 corp-CA1-CA，按一下**corp CA1 CA**。 

4. 在 結果 窗格的 狀態 欄中，確認 下列值，顯示**確定**:

    - **CA 憑證**
    - **AIA 位置 #1**
    - **CDP 位置 #1**   
  
  
> [!TIP]  
> 如果**狀態**不是任何項目**確定**，執行下列動作：  
> -   開啟您的 Web 伺服器，以確認憑證和憑證撤銷清單檔案已成功複製到共用上的共用。 如果它們並未成功複製到共用，修改您的複製命令與正確的檔案來源和目的地的共用並再次執行這些命令。  
> -   請確認您已在 [CA 擴充程式] 索引標籤上輸入 CDP 和 AIA 的正確位置。請確定沒有不含額外空格或其他您所提供的位置中的字元。  
> -   請確認正確的位置複製 CRL 和 CA 憑證，在您的 Web 伺服器上且位置符合您所提供的 CDP 和 AIA 位置，在 CA 上的位置。  
> -   確認正確設定的 CA 憑證和 CRL 的儲存位置的虛擬資料夾的權限。  
  


