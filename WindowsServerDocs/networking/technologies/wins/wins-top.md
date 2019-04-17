---
title: Windows 網際網路名稱服務」(WINS)
description: 本主題提供解除委任 WINS 及使用 DNS 名稱解析服務，您網路上的資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a3d132ada7b1ede83b046499058399a9da12190
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
#  <a name="windows-internet-name-service-wins"></a>Windows 網際網路名稱服務」(WINS)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

Windows 網際網路名稱服務」(WINS) 是舊版的電腦名稱登記和解析度服務電腦 NetBIOS 名稱地圖的 IP 位址。

如果不已經有 WINS 您網路上的部署，不要改為部署 WINS-，部署網域名稱系統 \(DNS\)。 DNS 也提供的電腦名稱登記和解析度服務，並透過 WINS 包含許多其他優點，例如與 Active Directory Domain Services 整合。

如需詳細資訊，請查看[網域名稱系統 」 (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

如果您已經擁有您網路上部署 WINS，建議您部署 DNS 並再解除 WINS。
