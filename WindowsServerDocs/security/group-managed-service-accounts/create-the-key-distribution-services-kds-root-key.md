---
title: "建立按鍵 Distribution 服務 KDS 根金鑰"
description: "Windows Server 安全性"
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
ms.openlocfilehash: 30075e56f3ca8e90a0655508efeacfcf2aaa0337
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-key-distribution-services-kds-root-key"></a>建立按鍵 Distribution 服務 KDS 根金鑰

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題中的 IT 專業人員告訴您如何使用 Windows PowerShell 產生網域控制站上建立的 Microsoft 金鑰 Distribution 服務 (kdssvc.dll) 根金鑰群組管理服務 AccountWindows Server 2012 中的密碼。

 Windows Server 2012 網域控制站 (DC) 需要根金鑰產生 gMSA 密碼。 10 小時的時間讓所有的網域控制站整合允許 gMSA 建立之前他們 AD 複寫建立等待網域控制站。 10 小時都是安全考量，以預防密碼代發生之前環境中的所有網域控制站的回答 gMSA 要求的功能。  如果您嘗試使用 gMSA 太快按鍵可能不已複寫所有 Windows Server 2012 網域控制站，因此密碼擷取可能會失敗 gMSA 主機嘗試擷取密碼。 使用 Dc 有限的複寫排程或是否有複寫問題時，也可能會發生 gMSA 密碼抓取失敗。

在成員資格**網域系統管理員**或**企業系統管理員」**群組或等最小，才能完成此程序。 適用於使用適當帳號和群組成員資格的詳細資訊，請查看[本機和網域預設群組](https://technet.microsoft.com/library/dd728026(WS.10).aspx)。

> [!NOTE]
> 64 位元架構，才能執行的 Windows PowerShell 命令可用來管理管理服務帳號群組。

#### <a name="to-create-the-kds-root-key-using-the-new-kdsrootkey-cmdlet"></a>若要建立使用 New-KdsRootKey cmdlet KDS 根金鑰

1.  Windows Server 2012 網域控制站，從工作列上執行的 Windows PowerShell。

2.  Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：

    **Add-KdsRootKey-EffectiveImmediately**

    > [!TIP]
    > 有效的時間參數，可用於提供按鍵所有網域控制站在使用之前會傳送至時間。 使用 Add-KdsRootKey-EffectiveImmediately 將根金鑰設為目標，將會使用 KDS 服務立即俠。 不過，其他 Windows Server 2012 網域控制站將無法再使用根鍵，直到複寫成功。

只有一個俠的測試環境，您可以建立 KDS 根金鑰和設定，以避免使用下列程序的主要代間隔等候過去的開始時間。 驗證的 4004 事件已登入 kds 事件登入。

#### <a name="to-create-the-kds-root-key-in-a-test-environment-for-immediate-effectiveness"></a>若要建立 KDS 根金鑰立即效率的測試環境中

1.  Windows Server 2012 網域控制站，從工作列上執行的 Windows PowerShell。

2.  Windows PowerShell Active Directory 模組的命令提示字元中，輸入下列命令，，然後按 ENTER 鍵：

    **$= 取得日期**

    **$b=$a.AddHours(-10)**

    **Add-KdsRootKey EffectiveTime $b**

    或者使用單一命令

    **Add-KdsRootKey EffectiveTime ((get-date).addhours(-10))**

## <a name="see-also"></a>也了
[開始使用群組管理帳號服務](getting-started-with-group-managed-service-accounts.md)


