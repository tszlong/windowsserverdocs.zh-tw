---
title: 將 CA 憑證和 CRL 複製到虛擬目錄
description: 瞭解如何將憑證授權單位單位的憑證撤銷清單和企業根 CA 憑證複製到 Web 服務器上的虛擬目錄，並確保 AD CS 的設定是否正確。
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.author: lizross
author: eross-msft
ms.date: 07/19/2018
ms.openlocfilehash: 47d0e72b06d60b8865356d74c041a26f62635046
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98038558"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>將 CA 憑證和 CRL 複製到虛擬目錄

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，將憑證授權單位單位的憑證撤銷清單和企業根 CA 憑證複製到 Web 服務器上的虛擬目錄，並確保 AD CS 設定正確。 執行下列命令之前，請確定將目錄和伺服器名稱取代為適用于您部署的目錄和伺服器名稱。

若要執行此程式，您必須是 **Domain Admins** 的成員。

#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>將憑證撤銷清單從 CA1 複製到 WEB1

1.  在 CA1 上，以系統管理員身分執行 Windows PowerShell，然後使用下列命令來發佈 CRL：

    - 輸入 `certutil -crl`，然後按 ENTER 鍵。

    - 若要將 CA1 憑證複製到 Web 服務器上的檔案共用，請輸入 `copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki` ，然後按 enter。

    - 若要將憑證撤銷清單複製到 Web 服務器上的檔案共用，請輸入 `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki` ，然後按 enter。

2.  若要確認您的 CDP 和 AIA 延伸模組位置是否已正確設定，請輸入 `pkiview.msc` ，然後按 enter。 Pkiview Enterprise PKI MMC 隨即開啟。

3.  在左窗格中，按一下您的 CA 名稱。<p>例如，如果您的 CA 名稱為 corp-CA1-CA，請按一下 [ **corp-CA1-ca**]。

4. 在結果窗格的 [狀態] 資料行中，確認下列值顯示 [ **確定]**：

    - **CA 憑證**
    - **AIA 位置 #1**
    - **CDP 位置 #1**


> [!TIP]
> 如果任何專案的 **狀態** 都不 **確定**，請執行下列動作：
> -   開啟 Web 服務器上的共用，確認已成功將憑證和憑證撤銷清單檔案複製到共用。 如果未成功將它們複製到共用，請使用正確的檔案來源和共用目的地來修改複製命令，然後再次執行命令。
> -   確認您已在 [CA 擴充功能] 索引標籤上輸入 CDP 和 AIA 的正確位置。確定您提供的位置中沒有額外的空格或其他字元。
> -   確認您已將 CRL 和 CA 憑證複製到 Web 服務器上的正確位置，且該位置符合您在 CA 上為 CDP 和 AIA 位置提供的位置。
> -   確認您已正確設定儲存 CA 憑證和 CRL 之虛擬資料夾的許可權。



