---
title: 動態主機設定通訊協定」(DHCP)
description: 本主題提供簡短的概觀的動態主機設定通訊協定 (DHCP) 在 Windows Server 2016。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 770cfd78c7b9a0e122bd9f9936623a73b56af808
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>動態主機設定通訊協定」(DHCP)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題適用於 Windows Server 2016 中 DHCP 簡短的概觀。

>[!NOTE]
>本主題中，除了下列 DHCP 文件會提供。
>
>- [DHCP 中的新功能](What-s-New-in-DHCP.md)
>- [部署 DHCP 使用 Windows PowerShell](dhcp-deploy-wps.md)

動態主機設定通訊協定」(DHCP) 就會自動與 IP 位址和其他設定的相關的資訊，例如子網路遮罩及 [預設閘道提供網際網路通訊協定」(IP) 主機 client 日伺服器通訊協定。 Rfc 2131 和 2132 年定義 DHCP 為網際網路工程設計工作推動 (小組 IETF) 標準根據在開機通訊協定（之前），通訊協定與 DHCP 共用許多實作詳細資料。 DHCP 可讓您以取得所的 TCP/IP 設定資訊，從 DHCP 伺服器主機。

Windows Server 2016 包含 DHCP 伺服器，這是您可以在您的網路租賃 IP 位址和其他資訊 DHCP 戶端部署選擇性網路伺服器角色。 所有 Windows 機器翻譯作業系統都包含 DHCP client TCP/IP 的一部分，DHCP client 支援預設。

## <a name="why-use-dhcp"></a>為何要使用 DHCP 嗎？

TCP 型網路上的每個裝置都必須具有獨特傳播 IP 位址來存取該網路，其資源。 DHCP，而新的電腦或移到另一個子網路中電腦的 IP 位址必須手動; 設定必須手動回收會從網路的電腦的 IP 位址。

使用 DHCP，此整個程序會自動執行，並集中管理。 DHCP 伺服器維護集區的 IP 位址，並租用任何 DHCP 式 client 地址，它網路開機。 因為的 IP 位址的動態（租用），而不是靜態（永久指派），不會再使用中的位址會自動返回集區中的配置。

網路系統管理員會建立 DHCP 伺服器維護 TCP/IP 設定的資訊並將戶端 DHCP 式租賃方案的形式提供位址設定。 DHCP 伺服器設定將資訊儲存在資料庫包含：

- 網路上所有用戶端 TCP/IP 設定參數有效。

- 有效的 IP 位址、維護指派給戶端，集區中，以及排除地址。

- 保留特定 DHCP 戶端相關聯的 IP 位址。 這可讓一致指派單一 DHCP client 單一 IP 位址。

- 租賃持續時間或的 IP 位址之前，可以使用租賃續約時所需的時間長度。

DHCP 式 client，時接受租賃提供的服務，會收到：

- 子網路連接到有效的 IP 位址。  
  
- 要求 DHCP 選項，其他參數 DHCP 伺服器設定為指派給戶端。 DHCP 選項的一些事情是路由器（預設閘道）、DNS 伺服器和 DNS 網域名稱。

## <a name="benefits-of-dhcp"></a>DHCP 的優點

DHCP 提供下列權益。

- **為了 IP 位址設定**。 DHCP 最小化造成手動 IP 位址設定，例如錯字，設定錯誤或地址而導致的一個以上的電腦 IP 位址指派在同一時間衝突。

- **減少網路管理**。 DHCP 包含下列功能，可減少網路管理：

    - 打造且自動化 TCP/IP 設定。

    - 定義 TCP/IP 設定，從中央位置的功能。

    - 若要使用 DHCP 選項指派一系列完整的其他 TCP/IP 設定值能力。

    - 有效的 IP 位址處理戶端必須會經常更新，那些可移植裝置 wireless 網路上移至不同位置，例如的變更。

    - 使用 DHCP 轉送代理程式，就不用在每一個子網路 DHCP 伺服器初始 DHCP 郵件轉寄。

