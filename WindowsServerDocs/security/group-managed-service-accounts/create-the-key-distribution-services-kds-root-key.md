---
title: 建立金鑰發佈服務 KDS 根金鑰
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e5db8f-1516-4d42-be0a-fa932f5588e9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fd335d61eae7cf753d09436d54f14c7d6004d643
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386899"
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>建立金鑰發佈服務 KDS 根金鑰

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題適用于 IT 專業人員，說明如何使用 Windows PowerShell 在網域控制站上建立 Microsoft 金鑰發佈服務（kdssvc）根金鑰，以在 Windows Server 2012 或更新版本中產生群組受管理的服務帳戶密碼。

網域控制站（DC）必須要有根金鑰，才能開始產生 gMSA 的密碼。 網域控制站最長會等待 10 小時 (從建立算起) 來允許所有網域控制站聚合它們的 AD 複寫，然後才允許建立 gMSA。 10 小時是一項安全措施，用來避免在環境中的所有 DC 都能夠回應 gMSA 要求之前就產生密碼。  如果您太快嘗試使用 gMSA，金鑰可能尚未複寫到所有網域控制站，因此當 gMSA 主機嘗試抓取密碼時，密碼抓取可能會失敗。 當使用具有有限複寫排程的 Dc 或發生複寫問題時，也可能會發生 gMSA 密碼抓取失敗。

> [!NOTE]
> 刪除和重新建立根金鑰可能會導致問題，因為索引鍵的快取會在刪除之後繼續使用舊金鑰。 如果重新建立根金鑰，就應該在所有網域控制站上重新啟動金鑰發佈服務（KDC）。

若要完成此程序，至少需要 **Domain Admins** 或 **Enterprise Admins** 群組的成員資格或同等權限。 如需使用適當帳戶和群組成員資格的詳細資料，請參閱 [本機與網域的預設群組](https://technet.microsoft.com/library/dd728026(WS.10).aspx)。

> [!NOTE]
> 若要執行用來管理群組「受管理的服務帳戶」的 Windows PowerShell 命令，必須有 64 位元架構。

#### <a name="to-create-the-kds-root-key-using-the-add-kdsrootkey-cmdlet"></a>若要使用 Add-kdsrootkey Cmdlet 來建立 KDS 根金鑰根金鑰

1.  在 Windows Server 2012 或更新版本的網域控制站上，從工作列執行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，然後按 ENTER 鍵：

    **新增-Add-kdsrootkey-EffectiveImmediately**

    > [!TIP]
    > Effective 時間參數可用來指定使用金鑰前先將金鑰傳播到所有 DC 的時間。 使用 Add-kdsrootkey-EffectiveImmediately 會將根金鑰新增至目標 DC，而 KDS 根金鑰服務將會立即使用該金鑰。 不過，在複寫成功之前，其他網域控制站將無法使用根金鑰。

對於只有一個 DC 的測試環境，您可以使用下列程序來建立 KDS 根金鑰，並設定一個過去的時間做為開始時間，以避開金鑰產生時的等候間隔。 請確認 4004 事件已記錄在 kds 事件記錄檔中。

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>在測試環境中建立立即生效的 KDS 根金鑰

1.  在 Windows Server 2012 或更新版本的網域控制站上，從工作列執行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，然後按 ENTER 鍵：

    **$a = 取得日期**

    **$b = $a. Time.addhours （-10）**

    **新增-Add-kdsrootkey-EffectiveTime $b**

    或使用單一命令

    **Add-kdsrootkey-EffectiveTime （（get-date） time.addhours （-10））**

## <a name="see-also"></a>另請參閱
[開始使用群組受管理的服務帳戶](getting-started-with-group-managed-service-accounts.md)


