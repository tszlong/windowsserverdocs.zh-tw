---
title: Windows 網際網路名稱服務 (WINS)
description: 本主題提供有關解除委任 WINS 的資訊，以及在您的網路上使用名稱解析服務的 DNS。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 54e3f69500ccb3dbf6b2dfe47f6dd035e0a20511
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315168"
---
#  <a name="windows-internet-name-service-wins"></a>Windows 網際網路名稱服務 (WINS)

>適用於：Windows Server (半年通道)、Windows Server 2016

Windows 網際網路名稱服務 (WINS) 是舊版電腦名稱登錄與解析服務，可將電腦 NetBIOS 名稱對應至 IP 位址。

如果您的網路上還沒有部署 WINS，請不要部署 WINS，而是將網域名稱系統部署 \(DNS\)。 DNS 也提供電腦名稱稱註冊和解析服務，並在 WINS 上包含許多額外的優點，例如與 Active Directory Domain Services 整合。

如需詳細資訊，請參閱[網域名稱系統（DNS）](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

如果您已在網路上部署 WINS，建議您部署 DNS，然後解除委任 WINS。
