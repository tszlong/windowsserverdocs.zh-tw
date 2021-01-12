---
title: 建立金鑰發佈服務 KDS 根金鑰
description: '瞭解如何使用 Windows PowerShell 在 Windows Server 2012 或更新版本中產生群組受管理的服務帳戶密碼，以在網域控制站上 ( # A0) 根金鑰建立 Microsoft 金鑰發佈服務。'
ms.topic: article
ms.assetid: 42e5db8f-1516-4d42-be0a-fa932f5588e9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 84b925cb44aa8f1da43ac71beff3341da3ff3880
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98112924"
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>建立金鑰發佈服務 KDS 根金鑰

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題適用于 IT 專業人員，說明如何使用 Windows PowerShell 在 Windows Server 2012 或更新版本中產生群組受管理的服務帳戶密碼，以在網域控制站上 ( # A0) 根金鑰建立 Microsoft 金鑰發佈服務。

網域控制站 (DC) 需要根金鑰，才能開始產生 gMSA 密碼。 網域控制站最長會等待 10 小時 (從建立算起) 來允許所有網域控制站聚合它們的 AD 複寫，然後才允許建立 gMSA。 10 小時是一項安全措施，用來避免在環境中的所有 DC 都能夠回應 gMSA 要求之前就產生密碼。  如果您太快就嘗試使用 gMSA，金鑰可能尚未被覆寫到所有網域控制站，因此當 gMSA 主機嘗試取得密碼時，密碼抓取可能會失敗。 使用具有有限複寫排程的 Dc 或發生複寫問題時，也可能發生 gMSA 密碼抓取失敗。

> [!NOTE]
> 刪除和重新建立根金鑰可能會導致問題，因為金鑰快取後，舊金鑰仍會繼續使用。 如果重新建立根金鑰，則應該在所有網域控制站上重新啟動金鑰散發服務 (KDC) 。

若要完成此程序，至少需要 **Domain Admins** 或 **Enterprise Admins** 群組的成員資格或同等權限。 如需使用適當帳戶和群組成員資格的詳細資訊，請參閱 [本機和網域預設群組](/previous-versions/orphan-topics/ws.10/dd728026(v=ws.10))。

> [!NOTE]
> 若要執行用來管理群組「受管理的服務帳戶」的 Windows PowerShell 命令，必須有 64 位元架構。

#### <a name="to-create-the-kds-root-key-using-the-add-kdsrootkey-cmdlet"></a>使用 Add-KdsRootKey Cmdlet 來建立 KDS 根金鑰根金鑰

1.  在 Windows Server 2012 或更新版本的網域控制站上，從工作列執行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，然後按 ENTER 鍵：

    **新增-Add-kdsrootkey-EffectiveImmediately**

    > [!TIP]
    > Effective 時間參數可用來指定使用金鑰前先將金鑰傳播到所有 DC 的時間。 使用 Add-KdsRootKey EffectiveImmediately 會將根機碼加入至 KDS 根金鑰服務將立即使用的目標 DC。 不過，在複寫成功之前，其他網域控制站將無法使用根金鑰。

對於只有一個 DC 的測試環境，您可以使用下列程序來建立 KDS 根金鑰，並設定一個過去的時間做為開始時間，以避開金鑰產生時的等候間隔。 請確認 4004 事件已記錄在 kds 事件記錄檔中。

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>在測試環境中建立立即生效的 KDS 根金鑰

1.  在 Windows Server 2012 或更新版本的網域控制站上，從工作列執行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，然後按 ENTER 鍵：

    **$a = Get-Date**

    **$b = $a. Time.addhours (-10)**

    **Add-Add-kdsrootkey-EffectiveTime $b**

    或使用單一命令

    **Add-kdsrootkey-EffectiveTime ( # B1 get-date) . time.addhours (-10) # B5**

## <a name="see-also"></a>另請參閱
[使用群組受管理的服務帳戶消費者入門](getting-started-with-group-managed-service-accounts.md)