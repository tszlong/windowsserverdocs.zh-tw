---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: "手動設定服務 Account 聯盟伺服器陣列"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5b5a8d198f93772903ea9b0a2b4b01075799bf0f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>手動設定服務 Account 聯盟伺服器陣列

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

如果您想要在 Active Directory 同盟服務 \(AD FS\) 中設定伺服器聯盟農場環境，您必須建立和專用的服務 account 設定在 Active Directory Domain Services \(AD DS\) 發電廠所在的位置。 您再每個聯盟中設定伺服器使用此帳號發電廠。 當您想要允許 client 驗證聯盟伺服器 AD FS 使用的 Windows 整合驗證的企業網路上的電腦時，您必須在組織中完成下列工作。  
  
> [!NOTE]  
> 您有伺服器整個聯盟陣列一次此程序中執行工作。 之後，當您使用 AD FS 聯盟伺服器設定精靈建立聯盟伺服器，您必須指定這個相同 account 在**服務 Account**頁面中每個聯盟伺服器發電廠精靈。  
  
#### <a name="create-a-dedicated-service-account"></a>建立專用的服務 account  
  
1.  建立專用的使用者 \ 日服務 account 位於組織的身分提供者 Active Directory 森林中。 這個 account 才能 Kerberos 驗證通訊協定發電廠案例中工作，並在每個聯盟伺服器允許 pass\ 透過驗證。 使用這個 account 只聯盟伺服器發電廠之目的。  
  
2.  編輯使用者 account 屬性，並選取 [**密碼永久**核取方塊。 這個動作會確保此服務帳號的功能，不會中斷網域密碼變更要求的結果。  
  
    > [!NOTE]  
    > 根據不到另一個驗證一部 Kerberos 門票存取嘗試透過 Windows 整合驗證時使用此專用 account 網路服務 account 會造成隨機錯誤。  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>若要設定的服務 account SPN  
  
1.  因為應用程式集區的 AD FS AppPool 為核對使用者 \ 日服務執行時，您必須設定該帳號服務主體名稱 \(SPN\) Setspn.exe command\ 列工具的網域中。 執行 Windows Server 2008 的電腦上的預設會安裝 Setspn.exe。 已加入使用者 \ 日服務 account 所在的相同網域的電腦上，執行下列命令：  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    例如，在所有聯盟伺服器都叢集在網域名稱系統 \(DNS\) 主機名稱 fs.fabrikam.com 和服務 account 名稱指定給 AD FS AppPool 稱為 adfs2farm 案例，輸入命令，如下所示，，然後按 ENTER 鍵：  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    它是才能完成此帳號的此一次的工作。  
  
2.  AD FS AppPool 身分變更服務過去之後，設定存取控制清單 \(ACLs\) SQL Server 資料庫，讓讀取這個新帳號，AD FS AppPool 可讀取原則的資料。  
  

