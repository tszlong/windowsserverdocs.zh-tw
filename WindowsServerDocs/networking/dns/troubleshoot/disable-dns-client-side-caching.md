---
title: 停用 DNS 用戶端上的 DNS 用戶端快取
description: 本文介紹如何在 DNS 用戶端上停用 DNS 用戶端快取。
manager: dcscontentpm
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: 74e7f3936418bd2f04234d07b2f600197e94b357
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87182344"
---
# <a name="disable-dns-client-side-caching-on-dns-clients"></a>停用 DNS 用戶端上的 DNS 用戶端快取

Windows 包含用戶端 DNS 快取。 用戶端 DNS 快取功能可能會產生錯誤的印象，指出 dns 「迴圈配置資源」負載平衡不會從 DNS 伺服器進行到 Windows 用戶端電腦。 當您使用 ping 命令來搜尋相同的 A 記錄功能變數名稱時，用戶端可能會使用相同的 IP 位址。  

## <a name="how-to-disable-client-side-caching"></a>如何停用用戶端快取

若要停止 DNS 快取，請執行下列其中一個命令：

```cmd
net stop dnscache
```

```cmd
sc servername stop dnscache
```


若要在 Windows 中永久停用 DNS 快取，請使用服務控制器工具或服務工具，將 DNS 用戶端服務啟動類型設定為 [**停用**]。 請注意，Windows DNS 用戶端服務的名稱可能也會顯示為「Dnscache」。 

> [!NOTE]
> 如果 DNS 解析程式快取已停用，用戶端電腦的整體效能會降低，且 DNS 查詢的網路流量也會增加。 

DNS 用戶端服務會將先前解析的名稱儲存在記憶體中，藉此優化 DNS 名稱解析的效能。 如果 DNS 用戶端服務已關閉，電腦仍然可以使用網路的 DNS 伺服器來解析 DNS 名稱。 

當 Windows 解析程式收到指向查詢的回應（不論是正面或負面）時，它會將該回應新增至其快取，進而建立 DNS 資源記錄。 解決器一律會先檢查快取，再查詢任何 DNS 伺服器。 如果 DNS 資源記錄位於快取中，則解析程式會使用快取中的記錄，而不是查詢伺服器。 這種行為可加速查詢，並減少 DNS 查詢的網路流量。 

您可以使用 ipconfig 工具來查看和排清 DNS 解析程式快取。 若要查看 DNS 解析程式快取，請在命令提示字元中執行下列命令：

```cmd
ipconfig /displaydns 
```

此命令會顯示 DNS 解析程式快取的內容，包括從 Hosts 檔案預先載入的 DNS 資源記錄，以及系統所解析的任何最近查詢的名稱。 經過一段時間之後，解析程式會捨棄快取中的記錄。 時間週期是由與 DNS 資源記錄相關聯的生存**時間（TTL）** 值所指定。 您也可以手動清除快取。 排清快取之後，電腦必須重新查詢電腦先前已解決的任何 DNS 資源記錄的 DNS 伺服器。 若要刪除 DNS 解析程式快取中的專案，請 `ipconfig /flushdns` 在命令提示字元中執行。

## <a name="using-the-registry-to-control-the-caching-time"></a>使用登錄來控制快取時間

> [!IMPORTANT]  
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/help/322756)，以免發生問題。

快取正面或負回應的時間長度取決於下列登錄機碼中的專案值：

**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\DNSCache\Parameters**

正回應的 TTL 是下列值的較小者： 

- 在查詢回應中所收到的解析程式所指定的秒數

- **MaxCacheTtl**登錄設定的值。

>[!Note]
>- 正面回應的預設 TTL 是86400秒（1天）。
>- 否定回應的 TTL 是在 MaxNegativeCacheTtl 登錄設定中指定的秒數。
>- 負值回應的預設 TTL 為5秒;在 Windows 10 之前的1703版中，預設值是900秒（15分鐘）。
如果您不想要快取否定回應，請將 MaxNegativeCacheTtl 登錄設定設為0。

若要在用戶端電腦上設定快取時間：

1. 啟動 [登錄編輯程式] （Regedit.exe）。

2. 在登錄中找出並按一下下列機碼：

   **HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\Dnscache\Parameters**

3. 在 [編輯] 功能表上，指向 [新增]，按一下 [DWORD 值]，然後新增下列登錄值：

   - 值名稱： MaxCacheTtl

     資料類型： REG_DWORD

     值資料：預設值86400秒。

     如果您將用戶端 DNS 快取中的最大 TTL 值降低為1秒，這會讓用戶端 DNS 快取的外觀變得已停用。

   - 值名稱： MaxNegativeCacheTtl

     資料類型： REG_DWORD

     數值資料：預設值為5秒。

     如果您不想要快取否定回應，請將值設定為0。

4. 輸入您要使用的值，然後按一下 [確定]。

5. 結束 [登錄編輯程式]。
