---
title: 動態主機設定通訊協定 (DHCP)
description: 本主題提供 Windows Server 2016 中的動態主機設定通訊協定 (DHCP) 的簡短總覽。
manager: brianlic
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 085c3955e88a9f03e50bc019e938650cf9e36a99
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948784"
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>動態主機設定通訊協定 (DHCP)

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來簡短介紹 Windows Server 2016 中的 DHCP。

> [!NOTE]
> 除了本主題之外，還提供下列 DHCP 檔。
>
> - [DHCP 的新功能](What-s-New-in-DHCP.md)
> - [使用 Windows PowerShell 部署 DHCP](dhcp-deploy-wps.md)

動態主機設定通訊協定 (DHCP) 是一種用戶端/伺服器通訊協定，它會自動提供網際網路通訊協定 (IP) 主機及其 IP 位址，以及其他相關的設定資訊，例如子網路遮罩和預設閘道。 Rfc 2131 和2132將 DHCP 定義為網際網路工程任務推動小組， (IETF) standard 根據啟動程式通訊協定 (BOOTP) ，這是 DHCP 共用許多執行詳細資料的通訊協定。 DHCP 可讓主機從 DHCP 伺服器取得必要的 TCP/IP 設定資訊。

Windows Server 2016 包含 DHCP 伺服器，這是選用的網路伺服器角色，您可以在網路上部署此角色，以將 IP 位址和其他資訊租用給 DHCP 用戶端。 所有以 Windows 為基礎的用戶端作業系統都包含 DHCP 用戶端作為 TCP/IP 的一部分，而 DHCP 用戶端預設為啟用。

## <a name="why-use-dhcp"></a>為何要使用 DHCP？

TCP/IP 網路上的每個裝置都必須有唯一的單點傳送 IP 位址，才能存取網路及其資源。 若未設定 DHCP，則必須手動設定新電腦或從某個子網移至另一個子網的 IP 位址;從網路移除的電腦 IP 位址必須以手動方式回收。

使用 DHCP 時，整個程式都是自動化的，而且會集中管理。 DHCP 伺服器會維護 IP 位址的集區，並在網路上啟動任何已啟用 DHCP 的用戶端時，將位址租用給該用戶端。 因為 IP 位址是動態的 (租用) 而不是靜態 (永久指派的) ，所以不再使用的位址會自動傳回給集區以重新配置。

網路系統管理員建立的 DHCP 伺服器會維護 TCP/IP 設定資訊，並以租用供應專案的形式提供位址設定給已啟用 DHCP 的用戶端。 DHCP 伺服器會將設定資訊儲存在資料庫中，包括：

- 網路上所有用戶端的有效 TCP/IP 設定參數。

- 在指派給用戶端的集區中維護的有效 IP 位址，以及排除的位址。

- 與特定 DHCP 用戶端相關聯的保留的 IP 位址。 這可將單一 IP 位址一致指派給單一 DHCP 用戶端。

- 租用期間，或在需要租用更新之前可以使用 IP 位址的時間長度。

啟用 DHCP 的用戶端在接受租用供應專案時，會收到：

- 它所連接之子網的有效 IP 位址。

- 要求的 DHCP 選項，也就是設定 DHCP 伺服器指派給用戶端的其他參數。 DHCP 選項的一些範例包括路由器 (預設閘道) 、DNS 伺服器和 DNS 功能變數名稱。

## <a name="benefits-of-dhcp"></a>DHCP 的優點

DHCP 提供下列優點。

- **可靠的 IP 位址** 設定。 DHCP 會將手動 IP 位址設定所造成的設定錯誤（例如印刷錯誤）或因將 IP 位址指派給一部以上電腦而造成的位址衝突降至最低。

- **減少網路管理**。 DHCP 包含下列功能，可減少網路管理：

    - 集中式和自動化 TCP/IP 設定。

    - 從中央位置定義 TCP/IP 設定的能力。

    - 透過 DHCP 選項來指派完整範圍的額外 TCP/IP 設定值的能力。

    - 有效處理必須經常更新之用戶端的 IP 位址變更，例如移動到無線網路上不同位置的可攜式裝置。

    - 使用 DHCP 轉送代理程式來轉送初始 DHCP 訊息，這樣就不需要在每個子網上使用 DHCP 伺服器。

