---
title: "管理 Tls (TLS)"
description: "Windows Server 安全性"
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
ms.date: 06/05/2017
ms.openlocfilehash: e26d1f833dbb7219251947f1cb3cb09def958aea
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="manage-transport-layer-security-tls"></a>管理 Tls (TLS)

## <a name="configuring-tls-cipher-suite-order"></a>設定 TLS 密碼套件訂單

不同的 Windows 版本支援不同的 TLS 密碼套件和的優先順序。 查看[在 TLS SSL (Schannel SSP) 的編碼器套件](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)的支援 Microsoft Schannel 者不同的 Windows 版本中的預設順序。

> [!NOTE] 
> 您也可以修改清單的密碼套件藉由 CNG 功能，請查看[設定優先順序 Schannel 密碼套件](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx)如需詳細資訊。

變更 TLS 密碼套件順序會在下一次開機生效。 重新開機或關機，直到現有的順序會生效。

> [!WARNING] 
> 更新的登錄設定預設優先順序訂購不支援，且可能會重設以維護更新。 

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>使用群組原則設定 TLS 密碼套件訂單

您可以使用 SSL 加密套件訂單群組原則設定來設定預設 TLS 密碼套件訂單。

1.  從群組原則管理主控台中，移至**電腦設定** > **系統管理範本]** > **網路** > **SSL 設定**。
2.  按兩下**SSL 加密套件訂單**，然後按一下 [**啟用**選項。
3.  以滑鼠右鍵按一下**SSL 加密套件**方塊中，然後選取 [**選取所有**的快顯功能表。

    ![群組原則設定](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4.  以滑鼠右鍵按一下 [選取的文字，然後選取 [**複製**的快顯功能表。
5.  文字貼上文字編輯器 notepad.exe 和更新的新密碼套件順序清單。

    > [!NOTE]
    > 嚴格以逗號分隔格式必須 TLS 密碼套件順序清單。 每個密碼套件字串將會以逗號 （，） 以右側的結尾。 

    > 此外，清單中的密碼套件僅限於 1023 字元。

6.  更換的清單中**SSL 加密套件**的更新排序的清單。
7.  按一下**[確定]**或**套用**。

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>使用 MDM，設定 TLS 密碼套件訂單

Windows 10 原則 CSP 支援 TLS 密碼套件的設定。 查看[密碼編譯日 TLSCipherSuites](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/policy-configuration-service-provider#cryptography-tlsciphersuites)如需詳細資訊。

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>使用 TLS PowerShell Cmdlet 設定 TLS 密碼套件訂單

TLS PowerShell 模組支援取得 TLS 密碼套件排序的清單，停用加密套件，讓密碼套件。 查看[TLS 模組](https://technet.microsoft.com/itpro/powershell/windows/tls/tls)如需詳細資訊。

## <a name="configuring-tls-ecc-curve-order"></a>設定 TLS ECC 曲線訂單 

開始使用 Windows 10 與 Windows Server 2016、 ECC 曲線訂單可以設定獨立套件順序的密碼。 如果 TLS 密碼清單中有橢圓曲線尾碼套件順序，它們將會覆寫的新橢圓曲線優先順序，當支援。 這可讓組織使用群組原則物件的相同的密碼套件順序設定不同版本的 Windows。

> [!NOTE]
> Windows 10 的前密碼套件字串已附加橢圓曲線判斷曲線優先順序。

### <a name="managing-windows-ecc-curves-using-certutil"></a>管理 Windows ECC 曲線使用 CertUtil

開始使用 Windows 10 與 Windows Server 2016，Windows 會提供橢圓曲線參數管理但命令列公用程式 certuil.exe。 橢圓曲線通常會儲存在 bcryptprimitives.dll。 使用 certutil.exe，系統管理員可以新增和分別移除曲線參數的 Windows。 Certutil.exe 安全地儲存曲線參數登錄中。 Windows 可以開始使用曲線參數曲線相關聯的名稱。    

#### <a name="displaying-registered-curves"></a>顯示且已的曲線

使用下列命令 certutil.exe 顯示曲線登記目前電腦的清單。

```powershell
certutil.exe –displayEccCurve
```

![Certutil 顯示曲線](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*若要顯示清單的且已曲線輸出圖 1 Certutil.exe。*

#### <a name="adding-a-new-curve"></a>新增新的曲線

建立並使用的受信任的其他實體研究曲線參數組織。  
想要在 Windows 中使用這些新曲線系統管理員必須新增曲線。  
使用下列命令 certutil.exe 新增曲線目前的電腦：

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- **CurveName**引數代表的曲線新增曲線參數了的名稱。
- **CurveParameters**引數代表的憑證，其中包含參數曲線您想要新增的檔案名稱。
- **CurveOid**引數代表包含您想要新增 （選擇性） 的曲線參數 OID 憑證的檔案名稱。
- **CurveType**引數代表值從命名曲線[EC 名曲線登錄](http://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8)（選擇性）。

![Certutil 新增曲線](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*新增使用 certutil.exe 曲線圖 2。*

#### <a name="removing-a-previously-added-curve"></a>移除曲線先前加入

系統管理員可以移除使用下列命令 certutil.exe 先前加入的曲線：

```powershell
Certutil.exe –deleteEccCurve curveName
```

系統管理員的身分曲線移除電腦之後，Windows 不能使用命名的曲線。

## <a name="managing-windows-ecc-curves-using-group-policy"></a>管理 Windows ECC 曲線使用群組原則

組織分散曲線參數企業版加入網域電腦使用群組原則和的群組原則的喜好設定登錄擴充功能。  
散布曲線的程序為：

1.  在 Windows 10 及 Windows Server 2016 上，使用**certutil.exe**以新增新的且已命名的曲線 windows。
2.  該相同電腦上，從左群組原則管理主控台 (GPMC)、 建立新的群組原則物件，並且進行編輯。
3.  瀏覽至**電腦設定 |喜好設定 |Windows 設定 |登錄**。  以滑鼠右鍵按一下**登錄**。 暫留在**新增]** ，然後選取**的收藏的項目**。 重新命名符合曲線名稱收藏的項目。 您將會建立在每一個登錄鍵一個登錄收集項目*HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters*。
4.  建立新的群組原則喜好設定登錄集合設定來新增新的**登錄項目**底下列出的每個登錄值的*HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters\ [curveName]*。
5.  將部署群組原則物件包含應該會收到新命名的曲線 Windows 10 與 Windows Server 2016 的電腦群組原則登錄收藏的項目。

    ![GPP 散發曲線](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *圖 3 所示使用 「 群組原則喜好設定來散發曲線*

## <a name="managing-tls-ecc-order"></a>管理 TLS ECC 訂單

開始使用 Windows 10 與 Windows Server 2016、 ECC 曲線訂單群組原則設定可設定預設 TLS ECC 曲線訂單。 使用一般 ECC 這個設定，組織，可以新增自己受信任的名作業系統曲線 （也就使用的 TLS 核准），並再新增這些名稱曲線曲線優先順序群組原則設定，以確保未來 TLS handshakes 中使用。 新曲線優先順序清單會變成作用中的下一步重新開機之後接收的原則設定。     

![GPP 散發曲線](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*圖 4 管理 TLS 弧形優先順序使用群組原則*


