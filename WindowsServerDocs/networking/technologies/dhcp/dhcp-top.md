---
title: 動態主機設定通訊協定 (DHCP)
description: 本主題提供的動態主機設定通訊協定 (DHCP) Windows Server 2016 中的簡短概觀。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7828d75d58ff328e826cb685899a76347ce56953
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812212"
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>動態主機設定通訊協定 (DHCP)

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題的 Windows Server 2016 中 DHCP 的簡短概觀。

> [!NOTE]
> 本主題中，除了下列 DHCP 文件使用。
>
> - [在 DHCP 中最新消息](What-s-New-in-DHCP.md)
> - [使用 Windows PowerShell 將 DHCP 部署](dhcp-deploy-wps.md)

動態主機設定通訊協定 (DHCP) 是一種用戶端/伺服器通訊協定，會自動提供網際網路通訊協定 (IP) 主機，其 IP 位址與其他的相關的組態資訊，例如子網路遮罩及預設閘道。 Rfc 2131 與 2132年定義 DHCP 做為網際網路工程任務推動小組 (IETF) 標準架構上開機通訊協定 (BOOTP)，與 DHCP 共用許多實作詳細資料的通訊協定。 DHCP 可讓主機以從 DHCP 伺服器取得必要的 TCP/IP 組態資訊。

Windows Server 2016 包含 DHCP 伺服器，也就是選用的網路伺服器角色，您可以部署您的租用 IP 位址的網路和其他資訊給 DHCP 用戶端上。 所有以 Windows 為基礎的用戶端作業系統包含 DHCP 用戶端 TCP/IP 的一部分，並且為 DHCP 用戶端預設會啟用。

## <a name="why-use-dhcp"></a>為何要使用 DHCP？

每個裝置上的 TCP 架構網路都必須具有唯一單點傳播 IP 位址來存取網路及其資源。 沒有 DHCP，用於新的電腦或從一個子網路移到另一個的電腦的 IP 位址必須以手動的方式; 設定從網路中移除的電腦的 IP 位址必須以手動方式回收。

使用 DHCP，這整個程序是自動化並集中管理。 DHCP 伺服器會維護的 IP 位址集區，並將位址租用給任何已啟用 DHCP 的用戶端在網路上啟動時。 因為 IP 位址是動態的 （租用），而不是靜態 （永久指派），所以不再使用中的位址都會自動回到重新配置的集區。

網路系統管理員建立與維護的 TCP/IP 組態資訊，並提供租用的供應項目表單中的 已啟用 DHCP 的用戶端的位址設定的 DHCP 伺服器。 DHCP 伺服器會包含資料庫中儲存的組態資訊：

- 有效的網路上的所有用戶端的 TCP/IP 組態參數。

- 有效的 IP 位址指派給用戶端，集區中維護，以及排除的位址。

- 特定的 DHCP 用戶端相關聯的保留的 IP 位址。 這可讓一致單一 IP 位址指派給單一的 DHCP 用戶端。

- 租用持續時間或為其 IP 位址之前可使用租用期更新為所需的時間長度。

已啟用 DHCP 的用戶端，在接受租用方案，接收：

- 它連接的子網路是有效的 IP 位址。  
  
- 要求的 DHCP 選項，也就是額外的參數設定為 DHCP 伺服器指派給用戶端。 DHCP 選項的一些範例是路由器 （預設閘道）、 DNS 伺服器及 DNS 網域名稱。

## <a name="benefits-of-dhcp"></a>DHCP 的優點

DHCP 提供下列優點。

- **可靠的 IP 位址設定**。 DHCP 降到最低的手動的 IP 位址組態，例如印刷錯誤所造成的組態錯誤，或解決 IP 位址指派給多部電腦同時所造成的衝突。

- **減少網路管理**。 DHCP 包括下列功能，可降低網路系統管理：

    - 集中式且自動化的 TCP/IP 設定。

    - 定義從中央位置的 TCP/IP 設定的能力。

    - 透過 DHCP 選項指派全系列的其他 TCP/IP 組態值的能力。

    - 必須如在無線網路移至不同位置的可攜式裝置經常更新的用戶端的 IP 位址會變更有效地處理。

    - 轉送使用 DHCP 轉接代理，而不需要針對每個子網路上的 DHCP 伺服器的初始 DHCP 訊息。

