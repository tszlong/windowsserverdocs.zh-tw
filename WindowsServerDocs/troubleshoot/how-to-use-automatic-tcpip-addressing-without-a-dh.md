---
title: 如何在沒有 DHCP 伺服器的情況下使用自動 TCP/IP 定址
description: 介紹如何在不使用 DHCP 伺服器的情況下使用自動 TCP/IP 定址。
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: troubleshooting
author: Deland-Han
ms.author: delhan
ms.reviewer: robsmi
ms.openlocfilehash: eb8a21fc999ce4f6c5953634dcfdc692b9dd0608
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948544"
---
# <a name="how-to-use-automatic-tcpip-addressing-without-a-dhcp-server"></a>如何在沒有 DHCP 伺服器的情況下使用自動 TCP/IP 定址

本文說明如何使用自動傳輸控制通訊協定/網際網路通訊協定 (TCP/IP) 定址，而不使用動態主機設定通訊協定 (DHCP) 伺服器存在於網路上。 本文「適用于」一節所列的作業系統版本有一項稱為「自動私人 IP 定址」 (APIPA) 的功能。 使用這項功能時，如果 DHCP 伺服器無法使用或不存在於網路上，Windows 電腦就可以將網際網路通訊協定指派 (IP) 位址。 這項功能可讓您在執行 TCP/IP 的 (LAN) 設定和支援較不困難的區域網路。

## <a name="more-information"></a>相關資訊

> [!IMPORTANT]
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/help/322756)，以免發生問題。

設定為使用 DHCP 的 Windows 電腦，可以在 DHCP 伺服器無法使用時，自動將網際網路通訊協定指派 (IP) 位址。 例如，這可能發生在沒有 DHCP 伺服器的網路上，或在網路上，如果 DHCP 伺服器暫時關閉進行維護。

網際網路指派的號碼授權單位 (IANA) 具有自動私人 IP 位址的保留169.254.255.255。 因此，APIPA 提供的位址不會與可路由傳送的位址發生衝突。

將 IP 位址指派給網路介面卡之後，電腦就可以使用 TCP/IP 與其他連線到相同 LAN 的電腦通訊，也可以將 IP 位址手動設定為 (169.254.，其中，x. y 是用戶端唯一識別碼) 位址範圍，子網路遮罩為255.255.0.0 的唯一識別碼。 請注意，電腦無法與其他子網上的電腦或未使用自動私人 IP 位址的電腦通訊。 預設會啟用自動私人 IP 位址。

在下列任何情況下，您可能會想要停用它：

- 您的網路使用路由器。

- 您的網路已連線到網際網路，但沒有 NAT 或 proxy 伺服器。

除非您已停用 DHCP 相關的訊息，否則當您在 DHCP 位址和自動私人 IP 位址之間進行變更時，DHCP 訊息會提供通知給您。 如果不小心停用 DHCP 訊息，您可以藉由將下列登錄機碼中的 PopupFlag 值值從00變更為01，將 DHCP 訊息重新開啟： `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VxD\DHCP`

請注意，您必須重新開機電腦，變更才會生效。 您也可以使用 Windows Millennium Edition、Windows 98 或 Windows 98 Second Edition 中的 Winipcfg 工具，判斷您的電腦是否使用 APIPA：

按一下 [開始]，按一下 [執行]，輸入 "winipcfg" (沒有引號) ，然後按一下 [確定]。 按一下 [詳細資訊]。 如果 [IP 自動程式位址] 方塊包含 169.254. 中的 IP 位址，則會啟用自動私人 IP 位址。 如果 [IP 位址] 方塊存在，則 [自動私人 IP 位址] 目前尚未啟用。
若為 Windows 2000、Windows XP 或 Windows Server 2003，您可以在命令提示字元中使用 IPconfig 命令，判斷電腦是否使用 APIPA：

按一下 [開始]，按一下 [執行]，輸入 "cmd" (沒有引號) ，然後按一下 [確定] 以開啟 MS-DOS 命令列視窗。 輸入 "ipconfig/all" (沒有引號) ，然後按 ENTER 鍵。 如果 [自動啟用自動啟用] 行顯示為 [是]，且 [自動安裝 IP 位址] 為 (169.254.，其中的 [x] 為用戶端唯一識別碼) ，則表示電腦使用的是 APIPA。 如果 [自動啟用自動啟用] 行顯示 [否]，表示電腦目前不在使用 APIPA。
您可以使用下列其中一種方法來停用自動私人 IP 位址。

您可以手動設定 TCP/IP 資訊，這會將 DHCP 全部停用。 您可以藉由編輯登錄來停用自動私人 IP 位址 (但不能停用 DHCP) 。 若要這麼做，您可以將 "IPAutoconfigurationEnabled" DWORD 登錄專案（其值為0x0）新增至 Windows Millennium Edition、Windows98 或 Windows 98 第二版的下列登錄機碼：  `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VxD\DHCP`

若為 Windows 2000、Windows XP 及 Windows Server 2003，則可以停用 APIPA，方法是將具有值0x0 的 "IPAutoconfigurationEnabled" DWORD 登錄專案新增至下列登錄機碼： `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\<Adapter GUID>`
> [!NOTE]
> **介面卡 GUID** 子機碼是電腦 LAN 介面卡 (GUID) 的全域唯一識別碼。

針對 IPAutoconfigurationEnabled DWORD 專案指定1的值將會啟用 APIPA，這是從登錄中省略此值時的預設狀態。

## <a name="examples-of-where-apipa-may-be-useful"></a>APIPA 可能有用的範例

### <a name="example-1-no-previous-ip-address-and-no-dhcp-server"></a>範例1：沒有先前的 IP 位址，且沒有 DHCP 伺服器

當您為 DHCP) 設定的 Windows 電腦 (正在初始化時，它會廣播三個或多個「探索」訊息。 如果 DHCP 伺服器在多個探索訊息廣播之後沒有回應，Windows 電腦就會將類別 B (APIPA) 位址指派給自己。 然後，Windows 電腦將會顯示一則錯誤訊息給電腦的使用者， (提供過去) 的 DHCP 伺服器尚未指派 IP 位址。 然後，Windows 電腦會每三分鐘傳送一次探索訊息，以嘗試建立與 DHCP 伺服器的通訊。

### <a name="example-2-previous-ip-address-and-no-dhcp-server"></a>範例2：先前的 IP 位址，沒有 DHCP 伺服器

電腦會檢查 DHCP 伺服器，如果找不到，則會嘗試聯絡預設閘道。 如果預設閘道回復，則 Windows 電腦會保留先前租用的 IP 位址。 但是，如果電腦未收到來自預設閘道的回應，或未指派任何回應，則會使用自動私人 IP 定址功能將 IP 位址指派給自己。 系統會向使用者顯示錯誤訊息，並每隔3分鐘傳送一次探索訊息。 一旦有 DHCP 伺服器行，就會產生一則訊息，指出已重新建立與 DHCP 伺服器的通訊。

### <a name="example-3-lease-expires-and-no-dhcp-server"></a>範例3：租用到期且沒有 DHCP 伺服器

以 Windows 為基礎的電腦會嘗試重新建立 IP 位址的租用。 如果 Windows 電腦找不到 DCHP 伺服器，它會在產生錯誤訊息之後，將 IP 位址指派給自己。 然後電腦會廣播四個探索訊息，每隔5分鐘就會重複整個程式，直到 DHCP 伺服器上線為止。 接著會產生一則訊息，指出已重新建立與 DHCP 伺服器的通訊。
