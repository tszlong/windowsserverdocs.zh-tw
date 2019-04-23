---
title: Windows 網際網路名稱服務 (WINS)
description: 本主題提供解除委任 WINS，並使用 DNS 進行名稱解析服務，在您網路上的相關資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bbc1871d29021aa3c99f14368a4711dac63f4cee
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843629"
---
#  <a name="windows-internet-name-service-wins"></a>Windows 網際網路名稱服務 (WINS)

>適用於：Windows Server （半年通道），Windows Server 2016

Windows 網際網路名稱服務 (WINS) 是舊版電腦名稱登錄與解析服務，可將電腦 NetBIOS 名稱對應至 IP 位址。

如果您還沒有在網路上部署 WINS，不要部署 WINS-相反地，部署網域名稱系統\(DNS\)。 DNS 也會提供電腦名稱登錄與解析服務，和透過 WINS，包含許多額外的好處，例如與 Active Directory 網域服務整合。

如需詳細資訊，請參閱[網域名稱系統 (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

如果您已在網路上部署 WINS，則建議您部署 DNS，並接著解除委任 WINS。
