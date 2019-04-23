---
title: 設定 NPS UDP 連接埠資訊
description: 您可以使用本主題，設定網路原則伺服器 (NPS)，用於遠端驗證撥號使用者服務 (RADIUS) 驗證與 Windows Server 2016 中的帳戶處理流量的連接埠。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44c20092180c47e97f1505271203f4491606bbcf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851969"
---
# <a name="configure-nps-udp-port-information"></a>設定 NPS UDP 連接埠資訊

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用下列程序設定網路原則伺服器 (NPS) 用於遠端驗證撥入使用者服務的連接埠\(RADIUS\)驗證和帳戶處理流量。

根據預設，NPS 會接聽的連接埠 1812年、 1813年、 1645年和 1646 用於這兩個網際網路通訊協定第 6 版上的 RADIUS 流量\(IPv6\)和所有已安裝的網路介面卡的 IPv4。

>[!NOTE]
>如果您解除安裝 IPv4 或 IPv6 網路介面卡上，NPS 不會監視已解除安裝的通訊協定的 RADIUS 流量。

1812 用於驗證而 1813 用於帳戶處理連接埠值會定義由 Internet Engineering Task Force RADIUS 標準連接埠\(IETF\) Rfc 2865 與 2866年中。 不過，根據預設，許多存取伺服器會使用帳戶處理要求的連接埠 1645 用於驗證要求，而 1646年。 無論您決定使用哪些連接埠號碼，請確定 NPS 與存取伺服器已設定為使用相同的項目。

>[重要]如果您不使用 RADIUS 預設連接埠號碼，您必須設定防火牆以允許新連接埠上的 RADIUS 流量的本機電腦上的例外狀況。 如需詳細資訊，請參閱 <<c0> [ 設定用於 RADIUS 流量的防火牆](nps-firewalls-configure.md)。

若要完成此程序，至少需要 **Domain Admins** 的成員資格或同等權限。

## <a name="to-configure-nps-udp-port-information"></a>若要設定 NPS UDP 連接埠資訊 

1. 開啟 NPS 主控台。
2. 以滑鼠右鍵按一下**網路原則伺服器**，然後按一下**屬性**。
3. 按一下 **連接埠**索引標籤，然後再檢查連接埠的設定。 如果您的 RADIUS 驗證與 RADIUS 帳戶處理 UDP 連接埠會因提供 （1812年與 1645 用於驗證，和 1813年及 1646 用於帳戶處理） 的預設值，輸入中的連接埠設定**驗證**和**Accounting**。
4. 若要使用多個通訊埠設定進行驗證或帳戶處理要求，請以逗號分隔的連接埠號碼。

如需管理 NPS 的詳細資訊，請參閱 <<c0> [ 管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。
