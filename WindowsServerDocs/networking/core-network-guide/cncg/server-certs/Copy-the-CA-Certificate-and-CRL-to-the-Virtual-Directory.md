---
title: 將憑證及 CRL 複製 Virtual Directory
description: 本主題是指南部署伺服器的憑證 802.1 X 的有線和無線部署的一部分
manager: brianlic
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e37bfce7f8cf33fd7fcb5e6227d783c28bd29d35
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>將憑證及 CRL 複製 Virtual Directory

>適用於：Windows Server（以每年次管道）、Windows Server 2016

將憑證撤銷清單及企業根憑證從您的憑證授權單位複製到您的網頁伺服器上 virtual directory 並確定 AD CS 已正確設定，您可以使用此程序。 之前，請執行下列命令，請先確定您在使用的是適用於您的部署取代 directory 和伺服器的名稱。  
  
若要執行此程序您必須的成員**網域系統管理員**。  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>將憑證撤銷清單 CA1 從複製到 WEB1  
  
1.  在 CA1，系統管理員的身分，以執行 Windows PowerShell，再發行 [使用下列命令 CRL:  
  
    - 輸入`certutil -crl`，然後按 ENTER 鍵。  
  
    - 若要將 CA 憑證複製到您的網頁伺服器上的檔案共用中，輸入`copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki`，然後按 ENTER 鍵。  
    - 若要在您的網頁伺服器上的檔案共用複製憑證撤銷清單，輸入`copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`，然後按 ENTER 鍵。  
  
2. 若要重新 AD CS，輸入`Restart-Service certsvc`，然後按 ENTER 鍵。  
  
2.  若要確認您 CDP 和 AIA 擴充功能的位置已正確設定，輸入`pkiview.msc`，然後按 ENTER 鍵。 Pkiview 企業 PKI MMC 開啟。  
  
3.  按一下您 CA 名稱。 例如，如果您的 CA 名稱 corp CA1 CA，請按一下**corp CA1 CA**。 在詳細資料窗格中，確認**狀態**值為**CA 憑證**、**AIA 位置 #1**，和**CDP 位置 #1**的所有**[確定]**。  
  
下圖描述 pkiview 結果窗格的 \ [確定 \ 狀態的所有項目。  
  
![adcs_pkiviewmedia/adcs_pkiview.png)  
  
> [!IMPORTANT]  
> 如果**狀態**中的任何項目不是**[確定]**，執行下列動作：  
> -   左在您的網頁伺服器驗證憑證和憑證撤銷列出檔案共用已順利複製共用。 如果已無法順利複製共用，修改複製命令與正確檔案來源分享目的地並重新執行指令。  
> -   確認您有任何額外空格或其他您所提供的位置的字元，請確定 CA 擴充功能] 索引標籤上輸入 CDP 和 AIA 正確的位置。  
> -   確認正確的位置複製 CRL 和 CA 憑證，在您的網頁伺服器，並且位置符合您 CA 上 CDP 和 AIA 位置所提供的位置。  
> -   請確認正確設定儲存 CA 憑證和 CRL virtual 資料夾的權限。  
  


