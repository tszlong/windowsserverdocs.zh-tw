---
title: 管理傳輸層安全性 (TLS)
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: 8053a14a74797cccce4c441d41f1f1623ba0ad6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879439"
---
# <a name="manage-transport-layer-security-tls"></a>管理傳輸層安全性 (TLS)

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows 10

## <a name="configuring-tls-cipher-suite-order"></a>設定 TLS 加密套件順序

不同 Windows 版本支援不同的 TLS 加密套件以及優先順序。 請參閱[TLS/ssl (安全通道 SSP) 的加密套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)Microsoft 安全通道提供者，在不同的 Windows 版本中支援的預設順序。

> [!NOTE] 
> 您也可以修改清單使用 CNG 函式的加密套件，請參閱[排列優先順序的安全通道加密套件](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx)如需詳細資訊。

變更 TLS 加密套件順序會在下次開機時生效。 重新啟動或關機，直到現有的訂單將會生效。

> [!WARNING] 
> 更新預設優先順序排序的登錄設定時，不支援，並與服務更新可能會重設。 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>使用群組原則設定 TLS 加密套件順序

若要設定預設 TLS 加密套件順序，您可以使用 SSL 加密套件順序群組原則設定。

1.  從 [群組原則管理] 主控台中，移至**電腦組態** > **系統管理範本** > **網路** >  **SSL 組態設定**。
2.  按兩下**SSL 加密套件順序**，然後按一下**已啟用**選項。
3.  以滑鼠右鍵按一下**SSL 加密套件**方塊，然後選取**選取所有**從快顯功能表。

    ![群組原則設定](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4.  以滑鼠右鍵按一下選取的文字，然後選取**複製**從快顯功能表。
5.  將文字貼到文字編輯器，例如 notepad.exe，並更新與新的加密套件順序 清單中。

    > [!NOTE]
    > TLS 加密套件順序清單必須嚴格的逗號分隔格式。 每個加密套件字串會以逗號 （，） 到右邊，它的結尾。 

    > 此外，加密套件的清單是限制為 1023 個字元。

6.  取代列入**SSL 加密套件**與更新的已排序清單。
7.  按一下 [**確定**] 或 [**套用**]。

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>使用 MDM 設定 TLS 加密套件順序

Windows 10 原則 CSP 支援 TLS 加密套件的設定。 請參閱[加密/TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites)如需詳細資訊。

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>使用 TLS PowerShell Cmdlet 設定 TLS 加密套件順序

TLS PowerShell 模組支援取得 TLS 加密套件的已排序的清單、 停用加密套件，並啟用加密套件。 請參閱[TLS 模組](https://technet.microsoft.com/itpro/powershell/windows/tls/tls)如需詳細資訊。

## <a name="configuring-tls-ecc-curve-order"></a>設定 TLS 的 ECC 曲線順序 

從 Windows 10 與 Windows Server 2016，ECC 曲線順序可以設定加密套件順序無關。 如果 TLS 加密套件順序有橢圓曲線尾碼的清單，它們將會覆寫新橢圓曲線優先順序中，啟用時。 這讓組織能夠使用群組原則物件，若要使用相同的加密套件順序設定不同版本的 Windows。

> [!NOTE]
> 在 Windows 10 之前已附加判定曲線優先順序橢圓曲線加密套件的字串。

### <a name="managing-windows-ecc-curves-using-certutil"></a>管理使用 CertUtil 的 Windows ECC 曲線

從 Windows 10 和 Windows Server 2016 開始，Windows 會提供管理透過命令列公用程式 certutil.exe 的橢圓曲線參數。 橢圓曲線參數會儲存在 bcryptprimitives.dll。 使用 certutil.exe，系統管理員可以加入和移除曲線參數與 Windows，分別。 Certutil.exe 在登錄中，請以安全地儲存的曲線參數。 Windows 可以開始使用名稱相關聯的曲線的曲線參數。    

#### <a name="displaying-registered-curves"></a>顯示已註冊的曲線

您可以使用下列 certutil.exe 命令，顯示一份註冊目前電腦的曲線。

```powershell
certutil.exe –displayEccCurve
```

![Certutil 顯示曲線](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*圖 1 Certutil.exe 輸出來顯示清單中的已註冊的曲線。*

#### <a name="adding-a-new-curve"></a>加入新的曲線

組織可以建立及使用其他受信任的實體所研究的曲線參數。  
想要在 Windows 中使用這些新的曲線的系統管理員必須將曲線。  
將曲線加入目前電腦使用下列的 certutil.exe 命令：

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- **CurveName**引數代表的曲線的曲線參數已加入在其下的名稱。
- **CurveParameters**引數所代表的憑證，其中包含您想要新增的曲線參數檔名。
- **CurveOid**引數所代表的檔案名稱的憑證，其中包含您想要新增 （選擇性） 的曲線參數的 OID。
- **CurveType**引數代表具名曲線，從十進位值[EC 具名曲線登錄](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8)（選擇性）。

![Certutil 中加入曲線](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*圖 2 新增使用 certutil.exe 的曲線。*

#### <a name="removing-a-previously-added-curve"></a>移除先前加入的曲線

系統管理員可以移除先前加入的曲線，使用下列 certutil.exe 命令：

```powershell
Certutil.exe –deleteEccCurve curveName
```

Windows 無法使用具名的曲線之後系統管理員會從電腦移除曲線。

## <a name="managing-windows-ecc-curves-using-group-policy"></a>使用群組原則管理 Windows ECC 曲線

組織可以散發企業，加入網域，電腦使用群組原則和群組原則喜好設定的登錄延伸模組的曲線參數。  
散發曲線的程序是：

1.  在 Windows 10 和 Windows Server 2016 上，使用**certutil.exe** Windows 中加入新的已註冊具名的曲線。
2.  從該相同的電腦，開啟群組原則管理主控台 (GPMC)，建立新的群組原則物件，並編輯它。
3.  瀏覽至**電腦設定 |喜好設定 |Windows 設定 |登錄**。  以滑鼠右鍵按一下**登錄**。 將滑鼠停留**的新**，然後選取**收集項**。 重新命名以符合曲線名稱的集合項目。 您將建立一個登錄收集項目，每個登錄機碼底下*HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*。
4.  藉由加入新設定新建立的群組原則喜好設定登錄收集**登錄項目**下方所列的每個登錄值*HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters\[curveName]*。
5.  部署群組原則物件，包含要應該會收到新的具名的曲線的 Windows 10 和 Windows Server 2016 電腦的群組原則登錄收集項目。

    ![GPP 散發曲線](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *圖 3 使用 「 群組原則喜好設定將曲線*

## <a name="managing-tls-ecc-order"></a>管理 TLS ECC 順序

從 Windows 10 和 Windows Server 2016 開始，設定可用的 ECC 曲線順序群組原則設定的預設 TLS ECC 曲線順序。 使用泛型的 ECC 這個設定，組織可以新增自己受信任的名為作業系統的曲線 （也就核准與 TLS 使用），然後新增至曲線優先順序群組原則設定，以確保它們可用於未來的 TLS 的 這些具名的曲線交握。 新的曲線優先順序清單上下次重新開機後接收的原則設定生效。     

![GPP 散發曲線](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*圖 4 管理 TLS 曲線使用群組原則的優先順序*


