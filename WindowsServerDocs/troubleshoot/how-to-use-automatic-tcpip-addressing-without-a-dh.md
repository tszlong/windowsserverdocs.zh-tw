---
title: 如何在沒有 DHCP 伺服器的情況下使用自動 TCP/IP 定址
description: 介紹如何在沒有 DHCP 伺服器的情況下使用自動 TCP/IP 定址。
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: troubleshoot
author: Deland-Han
ms.author: delhan
ms.reviewer: robsmi
ms.openlocfilehash: 46e7ae4a011c9a8d738dccd4fe103b49fee44c5b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935556"
---
# <a name="how-to-use-automatic-tcpip-addressing-without-a-dhcp-server"></a>如何在沒有 DHCP 伺服器的情況下使用自動 TCP/IP 定址

本文說明如何使用自動傳輸控制通訊協定/網際網路通訊協定 (TCP/IP) 定址，而不需要動態主機設定通訊協定， (DHCP) 伺服器存在於網路上。 本文「適用于」一節中所列的作業系統版本具有稱為「自動私人 IP 位址 (APIPA) 的功能。 有了這項功能，Windows 電腦就可以在 DHCP 伺服器無法使用或不存在於網路上時，指派網際網路通訊協定 (IP) 位址。 這項功能可讓您設定和支援小型區域網路 (LAN) 執行 TCP/IP 較不容易。

## <a name="more-information"></a>相關資訊

> [!IMPORTANT]
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/help/322756)，以免發生問題。

如果 DHCP 伺服器無法使用，則設定為使用 DHCP 的 Windows 電腦可以自動將網際網路通訊協定指派 (IP) 位址。 例如，這可能發生在沒有 DHCP 伺服器的網路上，或在網路上（如果 DHCP 伺服器暫時關閉進行維護）。

 (IANA) 的網際網路指派的號碼授權單位具有自動私人 IP 位址的保留169.254.255.255。 因此，APIPA 提供的位址保證不會與可路由的位址衝突。

將 IP 位址指派給網路介面卡後，電腦可以使用 TCP/IP 與連線到相同 LAN 且也設定為 APIPA 的任何其他電腦進行通訊，或將 IP 位址手動設為 (169.254.，其中的 x. y 是子網路遮罩為255.255.0.0 的用戶端唯一識別碼) 位址範圍。 請注意，電腦無法與其他子網的電腦或未使用自動私人 IP 位址的電腦通訊。 預設會啟用自動私人 IP 位址。

在下列任一情況下，您可能會想要停用它：

- 您的網路使用路由器。

- 您的網路已連線到網際網路，但沒有 NAT 或 proxy 伺服器。

除非您已停用 DHCP 相關的訊息，否則 DHCP 訊息會在 DHCP 位址和自動私人 IP 位址之間變更時，提供通知給您。 如果不小心停用 DHCP 訊息，您可以將下列登錄機碼中 PopupFlag 值的值從00變更為01，以重新開啟 DHCP 訊息：`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VxD\DHCP`

請注意，您必須重新開機電腦，變更才會生效。 您也可以使用 Windows Millennium Edition、Windows 98 或 Windows 98 Second Edition 中的 Winipcfg 工具，判斷您的電腦是否使用 APIPA：

按一下 [開始]，按一下 [執行]，輸入 "winipcfg" (不加引號) ，然後按一下 [確定]。 按一下 [詳細資訊]。 如果 [IP 自動設定位址] 方塊包含 169.254. 範圍內的 IP 位址，則會啟用 [自動私人 IP 位址]。 如果 [IP 位址] 方塊存在，則目前未啟用自動私人 IP 位址。
對於 Windows 2000、Windows XP 或 Windows Server 2003，您可以在命令提示字元中使用 IPconfig 命令，判斷電腦是否使用 APIPA：

依序按一下 [開始] 和 [執行]，輸入 "cmd" (不加引號) ，然後按一下 [確定] 以開啟 MS-DOS 命令列視窗。 輸入 "ipconfig/all" (不加引號) ，然後按 ENTER 鍵。 如果「自動安裝」行顯示「是」，而「自動設定 IP 位址」是 (169.254.，其中 x. y 是用戶端的唯一識別碼) ，則電腦會使用 APIPA。 如果「自動程式啟用」行顯示「否」，則表示電腦目前未使用 APIPA。
您可以使用下列其中一種方法來停用自動私人 IP 位址。

您可以手動設定 TCP/IP 資訊，這會完全停用 DHCP。 您可以藉由編輯登錄來停用自動私人 IP 位址 (，但不能使用 DHCP) 。 若要這麼做，您可以將 "IPAutoconfigurationEnabled" DWORD 登錄專案（值為0x0）新增至 Windows Millennium Edition、Windows98 或 Windows 98 Second Edition 的下列登錄機碼：`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VxD\DHCP`

對於 Windows 2000、Windows XP 和 Windows Server 2003，可以藉由將 "IPAutoconfigurationEnabled" DWORD 登錄專案（值為0x0）新增至下列登錄機碼來停用 APIPA：`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\<Adapter GUID>`
> [!NOTE]
> **介面卡 GUID**子機碼是電腦 LAN 介面卡 (GUID) 的全域唯一識別碼。

針對 IPAutoconfigurationEnabled DWORD 專案指定1的值將會啟用 APIPA，這是從登錄中省略此值時的預設狀態。

## <a name="examples-of-where-apipa-may-be-useful"></a>APIPA 可能有用的範例

### <a name="example-1-no-previous-ip-address-and-no-dhcp-server"></a>範例1：沒有先前的 IP 位址，也沒有 DHCP 伺服器

當您為 DHCP) 設定的 Windows 電腦 (進行初始化時，它會廣播三個或多個「探索」訊息。 如果在廣播數個探索訊息之後，DHCP 伺服器沒有回應，Windows 電腦就會將類別 B 指派 (APIPA) 位址。 然後，Windows 電腦會向電腦的使用者顯示錯誤訊息， (讓它從未從 DHCP 伺服器指派 IP 位址給過去的) 。 Windows 電腦會在嘗試建立與 DHCP 伺服器的通訊時，每隔三分鐘傳送一則探索訊息。

### <a name="example-2-previous-ip-address-and-no-dhcp-server"></a>範例2：先前的 IP 位址，沒有 DHCP 伺服器

電腦會檢查 DHCP 伺服器，如果找不到，則會嘗試聯繫預設閘道。 如果預設閘道回復，Windows 電腦就會保留先前租用的 IP 位址。 不過，如果電腦未收到來自預設閘道的回應，或未指派任何回應，則它會使用自動私人 IP 位址功能，將 IP 位址指派給自己。 系統會向使用者顯示錯誤訊息，並每隔3分鐘傳送一次探索訊息。 一旦 DHCP 伺服器開機後，就會產生一則訊息，指出已重新建立與 DHCP 伺服器的通訊。

### <a name="example-3-lease-expires-and-no-dhcp-server"></a>範例3：租用到期，沒有 DHCP 伺服器

以 Windows 為基礎的電腦會嘗試重新建立 IP 位址的租用。 如果 Windows 電腦找不到 DCHP 伺服器，它會在產生錯誤訊息之後，將 IP 位址指派給自己。 然後，電腦會廣播四個探索訊息，並在每隔5分鐘後重複整個程式，直到 DHCP 伺服器開始行為止。 接著會產生一則訊息，指出已重新建立與 DHCP 伺服器的通訊。
