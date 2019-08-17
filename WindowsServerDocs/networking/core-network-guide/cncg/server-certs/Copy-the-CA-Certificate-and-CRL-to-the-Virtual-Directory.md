---
title: 將 CA 憑證和 CRL 複製到虛擬目錄
description: 本主題是 802.1 X 有線和無線部署的部署伺服器憑證指南的一部分
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/19/2018
ms.openlocfilehash: 9dbe14bec1c39ab5b967276c4faf3e9fc5a9aae3
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546531"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>將 CA 憑證和 CRL 複製到虛擬目錄

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式, 將憑證授權單位單位的憑證撤銷清單和企業根 CA 憑證複製到 Web 服務器上的虛擬目錄, 並確保 AD CS 已正確設定。 執行下列命令之前, 請確定您已將目錄和伺服器名稱取代為適用于您部署的路徑。  
  
若要執行此程式, 您必須是**Domain Admins**的成員。  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>將憑證撤銷清單從 CA1 複製到 WEB1  
  
1.  在 CA1 上, 以系統管理員身分執行 Windows PowerShell, 然後使用下列命令發佈 CRL:  
  
    - 輸入 `certutil -crl`，然後按 ENTER 鍵。  

    - 若要將 CA1 憑證複製到 Web 服務器上的檔案共用, 請`copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki`輸入, 然後按 enter。  
    
    - 若要將憑證撤銷清單複製到 Web 服務器上的檔案共用, 請`copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`輸入, 然後按 enter。  
  
2.  若要確認您的 CDP 和 AIA 延伸模組位置已正確設定`pkiview.msc`, 請輸入, 然後按 enter。 Pkiview 企業 PKI MMC 隨即開啟。  
  
3.  在左窗格中, 按一下您的 CA 名稱。<p>例如, 如果您的 CA 名稱是 corp-CA1-CA, 請按一下 [ **corp-CA1-ca**]。 

4. 在 [結果] 窗格的 [狀態] 欄中, 確認下列值顯示 [**確定]** :

    - **CA 憑證**
    - **AIA 位置 #1**
    - **CDP 位置 #1**   
  
  
> [!TIP]  
> 如果任何專案的 [**狀態**] 不是 **[確定]** , 請執行下列動作:  
> -   開啟 Web 服務器上的共用, 確認已成功將憑證和憑證撤銷清單檔案複製到共用。 如果未成功複製到共用, 請使用正確的檔案來源和共用目的地來修改您的複製命令, 然後再次執行命令。  
> -   確認您已在 [CA 延伸模組] 索引標籤上輸入 CDP 和 AIA 的正確位置。請確定您提供的位置中沒有額外的空格或其他字元。  
> -   請確認您已將 CRL 和 CA 憑證複製到 Web 服務器上的正確位置, 而且該位置符合您在 CA 上為 CDP 和 AIA 位置提供的位置。  
> -   請確認您已針對儲存 CA 憑證和 CRL 的虛擬資料夾正確設定許可權。  
  


