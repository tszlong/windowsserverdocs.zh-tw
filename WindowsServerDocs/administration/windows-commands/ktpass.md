---
title: ktpass
description: Ktpass 命令的參考文章，它會在 AD DS 中設定主機或服務的伺服器主體名稱，並產生 keytab 檔案，其中包含服務的共用秘密金鑰。
ms.topic: article
ms.assetid: 47087676-311e-41f1-8414-199740d01444
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3bb523d35a1bbf2d15895201855a58e96ebb7772
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887643"
---
# <a name="ktpass"></a>ktpass

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory Domain Services (AD DS) 中設定主機或服務的伺服器主體名稱，並產生包含服務共用秘密金鑰的 keytab 檔案。 .keytab 檔案是根據 Kerberos 驗證通訊協定的 Massachusetts Institute of Technology (麻省理工學院，MIT) 實作。 Ktpass 命令列工具可讓支援 Kerberos 驗證的非 Windows 服務使用 Kerberos 金鑰發佈中心 (KDC) 服務所提供的互通性功能。

## <a name="syntax"></a>語法

```
ktpass
[/out <filename>]
[/princ <principalname>]
[/mapuser <useraccount>]
[/mapop {add|set}] [{-|+}desonly] [/in <filename>]
[/pass {password|*|{-|+}rndpass}]
[/minpass]
[/maxpass]
[/crypto {DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}]
[/itercount]
[/ptype {KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}]
[/kvno <keyversionnum>]
[/answer {-|+}]
[/target]
[/rawsalt] [{-|+}dumpsalt] [{-|+}setupn] [{-|+}setpass <password>]  [/?|/h|/help]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ------------|
| /out`<filename>` | 指定要產生的 Kerberos 版本 5. keytab 檔案的名稱。 **注意：** 這是您傳輸到不是執行 Windows 作業系統之電腦的 keytab 檔案，然後以您現有的 keytab 檔案（ */Etc/Krb5.keytab*）取代或合併。 |
| /princ `<principalname>` | 在表單中指定主體名稱 host/computer.contoso.com@CONTOSO.COM 。 **警告：** 這個參數會區分大小寫。 |
| /mapuser `<useraccount>` | 將**princ**參數所指定的 Kerberos 主體名稱對應至指定的網域帳戶。 |
| /mapop`{add|set}` | 指定如何設定對應屬性。<ul><li>**Add** -新增指定之本機使用者名稱的值。 這是預設值。</li><li>**設定**-針對指定的本機使用者名稱，設定資料加密標準 (DES) 唯一加密的值。</li></ul> |
| `{-|+}`desonly | 預設會設定 [僅限 DES 加密]。<ul><li>**+** 設定僅限 DES 加密的帳戶。</li><li>**-** 針對僅限 DES 加密的帳戶釋放限制。 **重要事項：** Windows 預設不支援 DES。</li></ul> |
| /in`<filename>` | 指定要從不是執行 Windows 作業系統的主機電腦讀取的 keytab 檔案。 |
| /pass`{password|*|{-|+}rndpass}` | 指定**princ**參數所指定之主體使用者名稱的密碼。 使用 `*` 來提示輸入密碼。 |
| /minpass | 將隨機密碼的最小長度設定為15個字元。 |
| /maxpass | 將隨機密碼的最大長度設定為256個字元。 |
| /crypto`{DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}` | 指定在 keytab 檔案中產生的金鑰：<ul><li>**DES-CBC-CRC** -用於相容性。</li><li>**DES-CBC-MD5** -嚴格地遵守 MIT 的執行，並用於相容性。</li><li>**RC4-HMAC-NT** -採用128位加密。</li><li>**AES256-sha1** -採用 AES256-CTS-HMAC-SHA1-96 加密。</li><li>   **AES128-sha1** -採用 AES128-CTS-HMAC-SHA1-96 加密。</li><li>**全部**-指出可以使用所有支援的密碼編譯類型。</li></ul><p>**注意：** 由於預設設定是以較舊的 MIT 版本為基礎，因此您應該一律使用 `/crypto` 參數。 |
| /itercount | 指定用於 AES 加密的反復專案計數。 預設會忽略非 AES 加密的**itercount** ，並將 AES 加密設定為4096。 |
| /ptype`{KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}` | 指定主體類型。<ul><li>**KRB5_NT_PRINCIPAL** - (建議) 的一般主體類型。</li><li>**KRB5_NT_SRV_INST** -使用者服務實例</li><li>  **KRB5_NT_SRV_HST** -主機服務實例</li></ul> |
| /kvno`<keyversionnum>` | 指定金鑰版本號碼。 預設值為 1。 |
| /answer`{-|+}` | 設定背景回應模式：<ul><li>**-** 自動使用 [**否**] 回應重設密碼提示。</li><li>**+** 使用 **[是]** 自動回答 [重設密碼] 提示。</li></ul> |
| /target | 設定要使用的網域控制站。 預設值是根據主體名稱來偵測網域控制站。 如果網域控制站名稱無法解析，則會出現一個對話方塊，提示您輸入有效的網域控制站。 |
| /rawsalt | 強制 ktpass 在產生金鑰時使用 rawsalt 演算法。 這是選擇性參數。 |
| `{-|+}dumpsalt` | 此參數的輸出會顯示用來產生金鑰的 MIT salt 演算法。 |
| `{-|+}setupn` | 除了服務主體名稱 (SPN) 以外， (UPN) 設定使用者主體名稱。 預設值是在 keytab 檔案中設定兩者。 |
| `{-|+}setpass <password>` | 在提供時設定使用者的密碼。 如果使用 rndpass，則會改為產生隨機密碼。 |
| /? | 顯示此命令的說明。 |

#### <a name="remarks"></a>備註

- 在不是執行 Windows 作業系統的系統上執行的服務，可以使用 AD DS 中的服務實例帳戶進行設定。 這可讓任何 Kerberos 用戶端使用 Windows Kdc 向不是執行 Windows 作業系統的服務進行驗證。

- **/Princ**參數不會由 ktpass 評估，並會依照提供的方式使用。 產生 Keytab 檔案時，不會檢查參數是否符合**userPrincipalName**屬性值的確切大小寫。 如果沒有完全相符的大小寫，則使用這個 Keytab 檔的區分大小寫 Kerberos 散發可能會有問題，而且在預先驗證期間甚至可能會失敗。 從 LDifDE 匯出檔案檢查並取出正確的**userPrincipalName**屬性值。 例如：

    ```
    ldifde /f keytab_user.ldf /d CN=Keytab User,OU=UserAccounts,DC=contoso,DC=corp,DC=microsoft,DC=com /p base /l samaccountname,userprincipalname
    ````

### <a name="examples"></a>範例

若要為不是執行 Windows 作業系統的主機電腦建立 keytab 檔案，您必須將主體對應至帳戶，並設定主機主體密碼。

1. 使用 [active directory**使用者和電腦**] 嵌入式管理單元，在未執行 Windows 作業系統的電腦上建立服務的使用者帳戶。 例如，建立名稱為*User1*的帳戶。

2. 使用**ktpass**命令來設定使用者帳戶的身分識別對應，方法是輸入：

    ```
    ktpass /princ host/User1.contoso.com@CONTOSO.COM /mapuser User1 /pass MyPas$w0rd /out machine.keytab /crypto all /ptype KRB5_NT_PRINCIPAL /mapop set
    ```

    > [!NOTE]
    > 您不能將多個服務實例對應到相同的使用者帳戶。

3. 在不是執行 Windows 作業系統的主機電腦上，將 keytab 檔案與 */Etc/Krb5.keytab*檔案合併。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
