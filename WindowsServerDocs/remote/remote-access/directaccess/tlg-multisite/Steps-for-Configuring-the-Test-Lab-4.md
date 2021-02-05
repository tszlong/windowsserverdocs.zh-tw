---
title: 設定測試實驗室的步驟
description: 瞭解如何設定遠端存取基礎結構、設定遠端存取服務器和用戶端，以及從網際網路和 Homenet 子網測試 DirectAccess 連線能力。
manager: brianlic
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: b78c92f46963291794f69d92a2ec8e04e0248edd
ms.sourcegitcommit: 658ee0e4cb1c25a6793afb5b64046000eaf6b773
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2021
ms.locfileid: "99589820"
---
# <a name="steps-for-configuring-the-test-lab"></a>設定測試實驗室的步驟

> 適用於：Windows Server (半年度管道)、Windows Server 2016

下列步驟說明如何設定遠端存取基礎結構、設定遠端存取服務器和用戶端，以及從網際網路和 Homenet 子網測試 DirectAccess 連線能力。

在此測試實驗室指南中，您將會執行下列步驟來建立多個遠端存取部署：

- [步驟1：完成基本](STEP-1-Complete-DirectAccess-Configuration.md)設定。 完成 [測試實驗室指南：使用混合的 IPv4 與 IPv6 示範 DirectAccess 單一伺服器安裝程式](https://go.microsoft.com/fwlink/p/?LinkId=237004)中的所有步驟。

- [步驟2：安裝和設定 ROUTER1](STEP-2-Install-and-Configure-ROUTER1.md)。 ROUTER1 會在公司網路和2公司網路的子網之間提供路由和轉送功能。

- [步驟3：安裝和設定 CLIENT2](STEP-3-Install-and-Configure-CLIENT2.md)。 CLIENT2 是 Windows 7 用戶端電腦，用來示範 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 遠端存取部署的回溯相容性。

- [步驟4：設定 APP1](STEP-4-Configure-APP1.md)。 使用 ROUTER1 做為預設閘道來設定 APP1，並以 2-DC1 作為替代 DNS 伺服器。

- [步驟5：設定 DC1](STEP-5-Configure-DC1.md)。 使用額外的 Active Directory 網站設定 DC1，並為 Windows 7 用戶端電腦設定額外的安全性群組。

- [步驟6：安裝和設定 2-DC1](STEP-6-Install-and-Configure-2-DC1.md)。 在多網站部署中，您有兩個或多個網域和網站。 2-DC1 提供 corp2.corp.contoso.com 網域的網域控制站和 DNS 服務。

- [步驟7：安裝和設定 2-APP1](STEP-7-Install-and-Configure-2-APP1.md)。 2-APP1 是網路和檔案伺服器，位於2公司網路。

- [步驟8：設定 INET1](STEP-8-Configure-INET1.md)。 INET1 會在此測試實驗室指南中模擬網際網路。 您必須設定可解析為 EDGE1 之公用 IP 位址的 DNS 專案。

- [步驟9：設定 EDGE1](STEP-9-Configure-EDGE1.md)。 在 EDGE1 上設定2公司網路的 DNS 伺服器和路由。

- [步驟10：安裝和設定 2-EDGE1](STEP-10-Install-and-Configure-2-EDGE1.md)。 多個遠端存取服務器在多網站部署中是必要的。 2-EDGE1 提供第二個網域的遠端存取服務。

- [步驟11：設定多網站部署](STEP-11-Configure-the-Multisite-Deployment.md)。 設定兩部遠端存取服務器之後，您可以設定您的多網站部署。

- [步驟12：測試 DirectAccess 連線能力](STEP-12-Test-DirectAccess-Connectivity.md)。 透過 EDGE1 和 2 EDGE1，從網際網路子網的兩部用戶端電腦測試 DirectAccess 連線能力。

- [步驟13：從 NAT 裝置後方測試 DirectAccess 連線能力](STEP-13-Test-DirectAccess-Connectivity-from-Behind-a-NAT-Device.md)。 從 NAT 裝置後方測試 DirectAccess 連線能力。

- [步驟14：設定設定的快照](STEP-14-Snapshot-the-Configuration.md)。 完成測試實驗室之後，請建立工作遠端存取多網站部署的快照，讓您稍後可以返回以測試其他案例。
