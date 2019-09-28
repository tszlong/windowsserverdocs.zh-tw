---
title: DHCP 子網選擇選項
description: 本主題提供 Windows Server 2016 中動態主機設定通訊協定（DHCP）的 DHCP 子網選擇選項的相關資訊。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.date: 08/17/2018
ms.openlocfilehash: 4718204fad49b23c84cc73b67164f34a803ddd86
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405757"
---
# <a name="dhcp-subnet-selection-options"></a>DHCP 子網選擇選項

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來取得新 DHCP 子網選擇選項的相關資訊。

DHCP 現在支援選項 82 \(sub-option 5 @ no__t-1。 您可以使用這些選項，讓 DHCP proxy 用戶端和轉送代理程式要求特定子網的 IP 位址，以及特定的 IP 位址範圍和範圍。  如需詳細資訊，請參閱**選項82子選項 5**：適用于[DHCPv4 之轉送代理程式資訊選項的 RFC 3527 連結選取子選項](https://tools.ietf.org/html/rfc3527)。

如果您使用設定 DHCP 選項82的 DHCP 轉送代理，子選項5，則轉送代理程式可以從特定的 IP 位址範圍要求 DHCP 用戶端的 IP 位址租用。


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>選項82子選項5：連結選取子選項

[轉送代理連結選擇] 子選項可讓 DHCP 轉送代理程式指定 IP 子網，供 DHCP 伺服器指派 IP 位址和選項。

DHCP 轉送代理程式通常會依賴閘道 IP 位址 \(GIADDR @ no__t-1 欄位來與 DHCP 伺服器通訊。 不過，GIADDR 受限於它的兩個操作函式：

1. 通知 DHCP 伺服器有關要求 IP 位址租用的 DHCP 用戶端所在的子網。
2. 通知 DHCP 伺服器使用 IP 位址來與轉送代理程式進行通訊。

在某些情況下，轉送代理程式用來與 DHCP 伺服器通訊的 IP 位址可能會不同于必須配置 DHCP 用戶端 IP 位址的 IP 位址範圍。 

在這種情況下，選項82的連結選取子選項會很有用，允許轉送代理程式明確陳述其所需的 IP 位址是以 DHCP v4 選項82子選項5的形式配置的子網。

> [!NOTE]
>
> 所有轉送代理程式 IP 位址（GIADDR）都必須是作用中 DHCP 領域 IP 位址範圍的一部分。 DHCP 領域 IP 位址範圍以外的任何 GIADDR 都會被視為 rogue 轉送，而 Windows DHCP 伺服器將不會認可來自這些轉送代理程式的 DHCP 用戶端要求。
>
> 您可以建立特殊範圍來「授權」轉送代理程式。 建立具有 GIADDR 的範圍（如果 GIADDR 的是連續 IP 位址，則會包含多個）、排除發佈的 GIADDR 位址，然後啟動範圍。 這會授權轉送代理程式，同時防止指派 GIADDR 位址。


### <a name="use-case-scenario"></a>使用案例

在此案例中，組織網路包含來賓使用者的 DHCP 伺服器和無線存取點 \(AP @ no__t-1。 來賓用戶端 IP 位址是由組織 DHCP 伺服器所指派-不過，由於防火牆原則的限制，DHCP 伺服器無法存取來賓無線網路或具有 broadcase 訊息的無線用戶端。

若要解決此限制，AP 會以連結選取子選項5設定，以指定要為來賓用戶端配置之 IP 位址的子網，而在 GIADDR 中也會指定內部介面的 IP 位址，而此位置會導致公司網路。
