---
title: 建立金鑰發佈服務 KDS 根金鑰
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3d5f7b46b28e6a2fbfafb664b69aebc8d34886fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867209"
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>建立金鑰發佈服務 KDS 根金鑰

>適用於：Windows Server （半年通道），Windows Server 2016

本主題適用於 IT 專業人員說明如何使用 Windows PowerShell 來產生 Windows Server 2012 中的群組受控服務帳戶密碼的網域控制站上建立 Microsoft 金鑰發佈服務 (kdssvc.dll) 的根目錄機碼。

 Windows Server 2012 網域控制站 (DC) 需要的根目錄機碼，若要開始產生 gMSA 密碼。 網域控制站最長會等待 10 小時 (從建立算起) 來允許所有網域控制站聚合它們的 AD 複寫，然後才允許建立 gMSA。 10 小時是一項安全措施，用來避免在環境中的所有 DC 都能夠回應 gMSA 要求之前就產生密碼。  如果您嘗試使用 gMSA 太快索引鍵可能沒有被複寫到所有 Windows Server 2012 網域控制站，並當 gMSA 主機嘗試抓取密碼時，擷取密碼可能會因此失敗。 使用有限的複寫排程，或是否有複寫問題的網域控制站時，也會發生 gMSA 密碼抓取失敗。

若要完成此程序，至少需要 **Domain Admins** 或 **Enterprise Admins** 群組的成員資格或同等權限。 如需使用適當帳戶和群組成員資格的詳細資料，請參閱 [本機與網域的預設群組](https://technet.microsoft.com/library/dd728026(WS.10).aspx)。

> [!NOTE]
> 若要執行用來管理群組「受管理的服務帳戶」的 Windows PowerShell 命令，必須有 64 位元架構。

#### <a name="to-create-the-kds-root-key-using-the-add-kdsrootkey-cmdlet"></a>若要建立使用 Add-kdsrootkey cmdlet 的 KDS 根金鑰

1.  在 Windows Server 2012 網域控制站，請從工作列執行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，然後按 ENTER 鍵：

    **Add-KdsRootKey -EffectiveImmediately**

    > [!TIP]
    > Effective 時間參數可用來指定使用金鑰前先將金鑰傳播到所有 DC 的時間。 使用 Add-kdsrootkey-EffectiveImmediately 就會將根目錄機碼新增至的目標 DC 會立即使用 KDS 服務。 不過，其他 Windows Server 2012 網域控制站將無法使用的根金鑰，等到複寫成功後。

對於只有一個 DC 的測試環境，您可以使用下列程序來建立 KDS 根金鑰，並設定一個過去的時間做為開始時間，以避開金鑰產生時的等候間隔。 請確認 4004 事件已記錄在 kds 事件記錄檔中。

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>在測試環境中建立立即生效的 KDS 根金鑰

1.  在 Windows Server 2012 網域控制站，請從工作列執行 Windows PowerShell。

2.  在 Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，然後按 ENTER 鍵：

    **$a=Get-Date**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey -EffectiveTime $b**

    或使用單一命令

    **Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))**

## <a name="see-also"></a>另請參閱
[開始使用群組受管理的服務帳戶](getting-started-with-group-managed-service-accounts.md)


