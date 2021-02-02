---
title: 管理傳輸層安全性 (TLS)
description: 瞭解如何管理傳輸層安全性。
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic
ms.date: 05/16/2018
ms.openlocfilehash: d3e06dbaf61b422779822cce208040601f2e56fa
ms.sourcegitcommit: 84b97d34d606b6bf4b6ec8760a93107f1b311428
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2021
ms.locfileid: "99245409"
---
# <a name="manage-transport-layer-security-tls"></a>管理傳輸層安全性 (TLS)

> 適用于： Windows Server (半年通道) 、Windows Server 2016、Windows 10

## <a name="configuring-tls-cipher-suite-order"></a>正在設定 TLS 加密套件順序

不同的 Windows 版本支援不同的 TLS 加密套件和優先權順序。 請參閱 [TLS/SSL (SCHANNEL SSP) 中的加密套件 ](/windows/win32/secauthn/cipher-suites-in-schannel) ，以取得不同 Windows 版本中 Microsoft Schannel 提供者所支援的預設順序。

> [!NOTE]
> 您也可以使用 CNG 函數修改加密套件清單，請參閱排列 [Schannel 加密套件的優先順序](/windows/win32/secauthn/prioritizing-schannel-cipher-suites) 以取得詳細資料。

TLS 加密套件順序的變更將會在下一次開機時生效。 在重新開機或關機之前，現有的訂單將會生效。

> [!WARNING]
> 不支援更新預設優先順序排序的登錄設定，而且可能會使用服務更新進行重設。

### <a name="configuring-tls-cipher-suite-order-by-using-group-policy"></a>使用群組原則設定 TLS 加密套件順序

您可以使用 SSL 加密套件順序群組原則設定來設定預設的 TLS 加密套件順序。

1. 從群組原則管理主控台中，移至 [**電腦** 設定]  >  **系統管理範本**[  >  **網路**  >  **SSL 設定**]。
2. 按兩下 [ **SSL 加密套件順序**]，然後按一下 [ **啟用** ] 選項。
3. 以滑鼠右鍵按一下 [ **SSL 加密套件** ] 方塊，然後從快顯功能表中選取 [ **全部選取** ]。

   ![群組原則設定](../media/Transport-Layer-Security-protocol/ssl-cipher-suite-order-gp-setting.png)

4. 以滑鼠右鍵按一下選取的文字，然後從快顯功能表中選取 [ **複製** ]。
5. 將文字貼到文字編輯器中，例如 notepad.exe，並使用新的加密套件順序清單進行更新。

   > [!NOTE]
   > TLS 加密套件順序清單必須採用嚴格的逗點分隔格式。 每個加密套件字串將以逗號 ( 結尾，) 到其右邊。
   >
   > 此外，加密套件清單的限制為1023個字元。

6. 以更新的已排序清單取代 **SSL 加密套件** 中的清單。
7. 按一下 [確定] 或 [套用]。

### <a name="configuring-tls-cipher-suite-order-by-using-mdm"></a>使用 MDM 設定 TLS 加密套件順序

Windows 10 原則 CSP 支援 TLS 加密套件的設定。 如需詳細資訊，請參閱 [密碼編譯/TLSCipherSuites](/windows/client-management/mdm/policy-csp-cryptography#cryptography-tlsciphersuites) 。

### <a name="configuring-tls-cipher-suite-order-by-using-tls-powershell-cmdlets"></a>使用 TLS PowerShell Cmdlet 設定 TLS 加密套件順序

TLS PowerShell 模組支援取得已排序的 TLS 加密套件清單、停用加密套件，以及啟用加密套件。 如需詳細資訊，請參閱 [TLS 模組](/powershell/module/tls/) 。

## <a name="configuring-tls-ecc-curve-order"></a>設定 TLS ECC 曲線順序

從 Windows 10 & Windows Server 2016 開始，可以設定與加密套件順序無關的 ECC 曲線順序。 如果 TLS 加密套件順序清單具有橢圓曲線尾碼，則會在啟用時以新的橢圓曲線優先順序順序來覆寫它們。 這可讓組織使用群組原則物件，以相同的加密套件順序來設定不同版本的 Windows。

> [!NOTE]
> 在 Windows 10 之前，會使用橢圓曲線附加加密套件字串來決定曲線的優先順序。

### <a name="managing-windows-ecc-curves-using-certutil"></a>使用 CertUtil 管理 Windows ECC 曲線

從 Windows 10 和 Windows Server 2016 開始，Windows 會透過命令列公用程式 certutil.exe 來提供橢圓曲線參數管理。
橢圓曲線參數會儲存在 bcryptprimitives.dll 中。 系統管理員可以使用 certutil.exe，分別新增和移除 Windows 的曲線參數。 Certutil.exe 會將曲線參數安全地儲存在登錄中。
Windows 可以依與曲線相關聯的名稱開始使用曲線參數。

#### <a name="displaying-registered-curves"></a>顯示已註冊的曲線

使用下列 certutil.exe 命令來顯示為目前電腦註冊的曲線清單。

```powershell
certutil.exe –displayEccCurve
```

![Certutil 顯示曲線](../media/Transport-Layer-Security-protocol/certutil-display-curves.png)

*[圖 1] Certutil.exe 輸出顯示已註冊曲線的清單。*

#### <a name="adding-a-new-curve"></a>新增曲線

組織可以建立及使用其他受信任實體所研究的曲線參數。
想要在 Windows 中使用這些新曲線的系統管理員必須加入曲線。
使用下列 certutil.exe 命令將曲線新增至目前的電腦：

```powershell
Certutil —addEccCurue curveName curveParameters [curveOID] [curveType]
```

- **CurveName** 引數代表加入曲線參數的曲線名稱。
- **CurveParameters** 引數代表憑證的檔案名，其中包含您想要新增之曲線的參數。
- **CurveOid** 引數代表憑證的檔案名，其中包含您想要新增 (選擇性) 的曲線參數 OID。
- **CurveType** 引數代表名為「[曲線](https://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-8)登錄」之命名曲線的十進位值， (選擇性) 。

![Certutil 加入曲線](../media/Transport-Layer-Security-protocol/certutil-add-curves.png)

*[圖 2] 使用 certutil.exe 加入曲線。*

#### <a name="removing-a-previously-added-curve"></a>移除先前加入的曲線

系統管理員可以使用下列 certutil.exe 命令移除先前新增的曲線：

```powershell
Certutil.exe –deleteEccCurve curveName
```

當系統管理員從電腦移除曲線之後，Windows 就無法使用指定的曲線。

## <a name="managing-windows-ecc-curves-using-group-policy"></a>使用群組原則管理 Windows ECC 曲線

組織可以使用群組原則和群組原則喜好設定登錄延伸模組，將曲線參數散發至企業、已加入網域的電腦。
分配曲線的程式如下：

1. 在 Windows 10 和 Windows Server 2016 上，使用 **certutil.exe** 將新註冊的命名曲線新增至 Windows。
2. 在同一部電腦上，開啟群組原則管理主控台 (GPMC) 、建立新的群組原則物件，然後加以編輯。
3. 流覽至 [電腦設定] **|喜好設定 |Windows 設定 |** 登錄。  以滑鼠右鍵按一下 [登錄 **]。** 將滑鼠停留在 **新** 的上方，然後選取 [ **收集項目**]。 重新命名收集項以符合曲線的名稱。 您將針對 *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters* 下的每個登錄機碼建立一個登錄收集項目。
4. 為 *HKEY_LOCAL_MACHINE\CurrentControlSet\Control\Cryptography\ECCParameters\[ curveName]* 底下列出的每個登錄值新增登錄 **專案**，以設定新建立的群組原則喜好設定登錄集合。
5. 將包含群組原則登錄收集項的群組原則物件部署到應接收新命名曲線的 Windows 10 和 Windows Server 2016 電腦。

    ![群組原則管理編輯器的 [喜好設定] 索引標籤的螢幕擷取畫面。](../media/Transport-Layer-Security-protocol/gpp-distribute-curves.png)

    *圖3使用群組原則喜好設定來分佈曲線*

## <a name="managing-tls-ecc-order"></a>管理 TLS ECC 順序

從 Windows 10 和 Windows Server 2016 開始，ECC 曲線順序群組原則設定可以用來設定預設的 TLS ECC 曲線順序。
使用一般 ECC 和這項設定時，組織可以將自己的受信任命名曲線 (，並已核准搭配 TLS) 使用到作業系統，然後將這些命名曲線新增至曲線優先順序群組原則設定，以確保在未來的 TLS 交握中使用它們。
接收原則設定之後，新的曲線優先順序清單會在下一次重新開機時變成作用中狀態。

![EEC 曲線順序對話方塊的螢幕擷取畫面。](../media/Transport-Layer-Security-protocol/gp-managing-tls-curve-priority-order.png)

*圖4使用群組原則管理 TLS 曲線優先順序*
