---
title: 在 DNS 中為 WEB1 建立別名 (CNAME) 記錄
description: 本主題是適用于 802.1 X 有線和無線部署的指南部署伺服器憑證的一部分
manager: brianlic
ms.topic: article
ms.assetid: bfae23f0-ae12-486b-94fe-50a137e141a5
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: c95efd5d9f0cbe7bbdba79d0971581f2c09ac53e
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950184"
---
# <a name="create-an-alias-cname-record-in-dns-for-web1"></a>\( \) 在 DNS 中為 WEB1 建立別名 CNAME 記錄

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，將 Web 服務器的別名標準名稱 \( CNAME \) 資源記錄新增至網域控制站上 DNS 中的區域。 使用 CNAME 記錄時，您可以使用多個名稱來指向單一主機，這樣做很容易就能在 \( 同一部電腦上裝載檔案傳輸通訊協定 FTP \) 伺服器和 Web 服務器。

因此，您可以免費使用您的 Web 服務器來裝載憑證授權單位單位 CA 的憑證撤銷清單 \( CRL， \) 也可以 \( \) 執行其他服務，例如 FTP 或網頁伺服器。

當您執行此程式時，請以適用于您部署的值取代 **別名名稱** 和其他變數。

若要執行此程式，您必須是 **Domain Admins** 的成員。

## <a name="to-add-an-alias-cname-resource-record-to-a-zone"></a>將別名 \( CNAME \) 資源記錄新增至區域

>[!NOTE]
>若要使用 Windows PowerShell 來執行此程式，請參閱 [DnsServerResourceRecordCName](/powershell/module/dnsserver/add-dnsserverresourcerecordcname?view=winserver2012r2-ps)。

1.  在 DC1 的伺服器管理員中，按一下 [ **工具** ]，然後按一下 [ **DNS**]。 [DNS 管理員 Microsoft Management Console (MMC) 隨即開啟。

2.  在主控台樹中，按兩下 [**正向對應區域**]，以滑鼠右鍵按一下您要新增別名資源記錄的正向對應區域，然後按一下 [**新增別名 \( CNAME \)**]。 [ **新增資源記錄** ] 對話方塊隨即開啟。

3.  在 [ **別名名稱**] 中，輸入別名名稱 **pki**。

4.  當您輸入 **別名名稱** 的值時，此對話方塊中的 **完整 \( 功能變數名稱 \) FQDN** 會自動填滿。 例如，如果您的別名名稱是「pki」，而您的網域是 corp.contoso.com，則系統會自動為您填入值 **pki.corp.contoso.com** 。

5.  在 [ **\( \) 目標主機的完整功能變數名稱 fqdn**] 中，輸入您的 Web 服務器的 fqdn。 例如，如果您的 Web 服務器命名為 WEB1，而您的網域是 corp.contoso.com，請輸入 **WEB1.corp.contoso.com**。

6.  按一下 **[確定]** ，將新的記錄新增至區域。