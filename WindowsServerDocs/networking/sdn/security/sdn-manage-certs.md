---
title: 管理軟體定義網路的憑證
description: 您可以使用本主題以了解如何管理網路控制器 Northbound 和 Southbound 通訊的憑證，當您部署軟體定義網路 (SDN) 在 Windows Server 2016 Datacenter。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: d29a98e24b475c38fee61972bf9efbd5a2528974
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446267"
---
# <a name="manage-certificates-for-software-defined-networking"></a>管理軟體定義網路的憑證

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來了解如何管理網路控制器 Northbound 和 Southbound 通訊的憑證，當您部署軟體定義網路\(SDN\)在 Windows Server 2016 Datacenter 及您使用系統Center Virtual Machine Manager \(SCVMM\)作為 SDN 管理用戶端。

>[!NOTE]
>如需網路控制站的概觀資訊，請參閱[網路控制卡](../technologies/network-controller/Network-Controller.md)。

如果您不使用 Kerberos 來保護網路控制站通訊，您可以使用 X.509 憑證進行驗證、 授權和加密。

Windows Server 2016 Datacenter 中的 SDN 支援這兩個自我\-簽章和憑證授權單位\(CA\)-簽署 X.509 憑證。 本主題提供建立這些憑證，並將其套用的保護與管理用戶端的網路控制器 Northbound 通訊通道的逐步指示與 Southbound 通訊與網路裝置，例如軟體負載平衡器\(SLB\)。
.
當您使用憑證\-型驗證，您必須註冊在網路控制卡節點上使用下列方式的一個憑證。

1. 加密 Northbound 通訊使用安全通訊端層\(SSL\)之間網路控制卡節點和管理用戶端，例如 System Center Virtual Machine Manager。
2. 網路控制卡節點與 Southbound 裝置和服務，例如 HYPER-V 主機和軟體負載平衡器之間的驗證\(SLBs\)。

## <a name="creating-and-enrolling-an-x509-certificate"></a>建立和註冊的 X.509 憑證

您可以建立並註冊自我\-簽署憑證或由 CA 所發出的憑證。

>[!NOTE]
>當您使用 SCVMM 來部署網路控制站時，您必須指定用來加密 Northbound 通訊在網路控制站服務範本的組態期間的 X.509 憑證。

憑證設定必須包含下列值。

- 值**RestEndPoint**文字方塊中必須是網路控制站完整網域名稱\(FQDN\)或 IP 位址。 
- **RestEndPoint**值必須符合主體名稱\(常見的名稱、 CN\) X.509 憑證。

### <a name="creating-a-self-signed-x509-certificate"></a>建立自我\-簽署的 X.509 憑證

您可以建立自我簽署的 X.509 憑證，並將它匯出的私密金鑰\(受到密碼保護\)依照這些步驟的單一\-節點和多個\-節點網路控制站的部署.

當您建立自我\-簽署憑證，您可以使用下列指導方針。

- 您可以使用網路控制站的 REST 端點的 IP 位址的 DnsName 參數-但這因為它需要的網路控制卡節點都在單一管理子網路內不建議使用\(在單一機架上，例如\)
- 針對多個節點 NC 部署，您指定的 DNS 名稱會變成網路控制站叢集的 FQDN \(DNS 主機 A 記錄會自動建立。\) 
- 針對單一節點網路控制卡部署，DNS 名稱可以是網路控制站的主機名稱，後面接著完整網域名稱。

#### <a name="multiple-node"></a>多個節點

您可以使用[New-selfsignedcertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令來建立自我\-簽署憑證。

**語法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**使用方式範例**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>單一節點

您可以使用[New-selfsignedcertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令來建立自我\-簽署憑證。

**語法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**使用方式範例**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>建立 CA\-簽署的 X.509 憑證

若要建立使用 CA 的憑證，您必須已部署公開金鑰基礎結構\(PKI\)與 Active Directory 憑證服務\(AD CS\)。 

>[!NOTE]
>您可以使用協力廠商 Ca 或工具，例如 openssl，網路控制站，以建立使用的憑證，不過本主題中的指示僅適用於 AD CS。 若要了解如何使用協力廠商 CA 或工具，請參閱您使用軟體的說明文件。

建立 ca 的憑證包含下列步驟。

1. 您或貴組織的網域或安全性系統管理員設定憑證範本
2. 您或貴組織的網路控制站的系統管理員或 SCVMM 系統管理員從 CA 要求新憑證。

#### <a name="certificate-configuration-requirements"></a>憑證設定需求

而下一個步驟中，您要設定憑證範本，請確定您所設定的範本包含下列必要的項目。

1. 憑證主體名稱必須是 HYPER-V 主機的 FQDN
2. 憑證必須放在本機電腦個人存放區 (My-cert: \localmachine\my)
3. 此憑證必須具有這兩個伺服器驗證 (EKU:1.3.6.1.5.5.7.3.1) 」 和 「 用戶端驗證 (EKU:1.3.6.1.5.5.7.3.2) 應用程式原則。

>[!NOTE]
>如果個人\(我 – cert: \localmachine\my\)在 Hyper-v 上的憑證存放區\-主機有一個以上的 X.509 憑證的主體名稱 (CN) 與主機完整網域名稱\(FQDN\)，請確定將供 SDN 的憑證具有 OID 1.3.6.1.4.1.311.95.1.1.1 的其他自訂增強金鑰使用方法屬性。 否則，網路控制站與主機之間的通訊可能會無法運作。

#### <a name="to-configure-the-certificate-template"></a>若要設定憑證範本
  
>[!NOTE]
>執行此程序之前，您應該檢閱憑證需求和可用的憑證範本，在 [憑證範本] 主控台中。 您可以修改現有的範本或建立現有範本的複本，然後修改範本的複本。 建議您將建立現有範本的複本。

1. 在伺服器上安裝 AD CS 是，在 [伺服器管理員] 中，按一下**工具**，然後按一下**憑證授權單位**。 憑證授權單位的 Microsoft Management Console \(MMC\)隨即開啟。 
2. 在 MMC 中，按兩下 CA 名稱，以滑鼠右鍵按一下**憑證範本**，然後按一下**管理**。
3. 憑證範本主控台隨即開啟。 在 [詳細資料] 窗格中，會顯示所有的憑證範本。
4. 在 [詳細資料] 窗格中，按一下您想要複製的範本。
5.  按一下 **動作**功能表，然後再按一下**複製範本**。 範本**屬性**對話方塊隨即開啟。
6.  在範本中**屬性**對話方塊的 **主體名稱**索引標籤上，按一下 **在要求中提供**。 \(這是必要設定網路控制站的 SSL 憑證。\)
7.  在範本中**屬性**對話方塊的 **處理要求**索引標籤上，確定**允許私密金鑰匯出**已選取。 也請確認**簽章和加密**選取用途。
8.  在範本中**屬性**對話方塊的 **擴充功能**索引標籤上，選取**金鑰使用方法**，然後按一下**編輯**。
9.  在 **簽章**，請確認**數位簽章**已選取。
10.  在範本中**屬性**對話方塊的 **擴充功能**索引標籤上，選取**應用程式原則**，然後按一下**編輯**。
11.  在**應用程式原則**，請確認**用戶端驗證**並**伺服器驗證**列出。
12.  儲存憑證範本的複本的唯一名稱，例如**網路控制站範本**。

#### <a name="to-request-a-certificate-from-the-ca"></a>若要向 CA 要求憑證

您可以使用 憑證嵌入式管理單元要求憑證。 您可以要求任何類型的預先設定和處理憑證要求的 CA 系統管理員所提供的憑證。

**使用者**或本機**系統管理員**完成此程序所需的最小的群組成員資格。

1. 開啟憑證嵌入式管理單元的電腦。
2. 在主控台樹狀目錄中，按一下**憑證\(本機電腦\)** 。 選取 **個人**憑證存放區。
3. 在上**動作**功能表上，指向 * * 所有工作<strong>，然後按一下 * * 要求新憑證</strong>啟動 憑證註冊精靈。 按一下 [下一步]  。
4. 選取 [**系統管理員設定**憑證註冊原則，然後按一下**下一步]** 。
5. 選取  **Active Directory 註冊原則**\(根據您在上一節中設定的 CA 範本\)。
6. 依序展開**詳細資料**區段，然後設定下列項目。
   1. 請確認**金鑰使用方法**同時包含<strong>數位簽章 * * 和 * * 金鑰編密</strong>。
   2. 請確認**應用程式原則**同時包含**伺服器驗證** \(1.3.6.1.5.5.7.3.1\)並**用戶端驗證**\(1.3.6.1.5.5.7.3.2\)。
7. 按一下 [內容]  。
8. 在上**主旨**索引標籤**主體名稱**，請在**型別**，選取**一般名稱**。 在 [值] 中，指定**網路控制站的 REST 端點**。
9. 按一下 [套用]  ，然後按一下 [確定]  。
10. 按一下 **\[註冊\]** 。

在憑證 MMC 中，按一下要檢視您已從憑證的 CA 註冊的憑證之個人存放區。

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>匯出，並將憑證複製到 SCVMM 程式庫

建立自我之後\-簽署或 CA\-簽署憑證，您必須匯出憑證的私密金鑰\(.pfx 格式\)和不含私密金鑰\(Base-64.cer 格式\)從 [憑證] 嵌入式管理單元。 

您接著必須將複製到兩個匯出的檔案**ServerCertificate.cr**並**NCCertificate.cr**匯入 NC 服務範本時的時間所指定的資料夾。

1. 開啟憑證嵌入式管理單元 (certlm.msc) 並在本機電腦個人憑證存放區中找出憑證。
2. 右\-按一下憑證，再按一下**所有工作**，然後按一下**匯出**。 \[憑證匯出精靈\] 隨即開啟。 按一下 [下一步]  。
3. 選取  **是**，匯出私密金鑰 選項，按一下**下一步**。
4. 選擇**個人資訊交換-PKCS #12 (。PFX)** 並接受預設值**包含憑證路徑中的所有憑證**的話。
5. 指派使用者/群組以及您要匯出的憑證的密碼，請按一下**下一步**。
6. 在要匯出頁面的檔案，瀏覽您要放置匯出的檔案，的位置，並提供名稱。
7. 同樣地，以匯出憑證。CER 格式。 注意:若要匯出至。CER 格式，取消核取 [是，匯出私密金鑰] 選項。
8. 複製。PFX 到 ServerCertificate.cr 資料夾。
9. 複製。CER 檔案到 NCCertificate.cr 資料夾。

完成之後，重新整理 SCVMM 程式庫中的這些資料夾，並確定您已複製這些憑證。 繼續執行的網路控制站服務範本設定和部署。

## <a name="authenticating-southbound-devices-and-services"></a>驗證 Southbound 裝置和服務 

主機與 SLB MUX 裝置網路控制器通訊會使用憑證進行驗證。 與主機的通訊是透過 OVSDB 通訊協定，透過 WCF 通訊協定與 SLB MUX 裝置通訊時。

### <a name="hyper-v-host-communication-with-network-controller"></a>與網路控制卡的 HYPER-V 主機通訊

透過 OVSDB 的 HYPER-V 主機的通訊，網路控制站必須出示的憑證到主機電腦。 根據預設，SCVMM 會挑選網路控制卡上設定的 SSL 憑證，並使用 southbound 與主機的通訊。

這是為什麼 SSL 憑證必須設定用戶端驗證 EKU 的原因。 此憑證已在 「 伺服器 」 REST 資源\(HYPER-V 主機會表示為伺服器資源的網路控制卡中\)，而且可以執行 Windows PowerShell 命令來檢視**取得 NetworkControllerServer**。

以下是在伺服器 REST 資源的部分範例。

      "resourceId": "host31.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "host31.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

進行相互驗證，HYPER-V 主機也必須有憑證才能與網路控制器通訊。 

您可以註冊之憑證的憑證授權單位\(CA\)。 如果主機電腦上找不到基礎的 CA 憑證，SCVMM 會建立自我簽署的憑證，並在主機電腦上為它佈建。

網路控制站和憑證必須受到彼此信任的 HYPER-V 主機。 HYPER-V 主機憑證的根憑證必須存在於本機電腦，反之亦然，網路控制器受信任的根憑證授權單位存放區。 

當您使用自我\-簽署憑證，SCVMM 可確保所需的憑證會出現在本機電腦的受信任的根憑證授權單位存放區。 

如果您使用 HYPER-V 主機的基礎的 CA 憑證，您需要確保 CA 根憑證會出現在網路控制站的受信任的根憑證授權單位存放區，本機電腦上。

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>與網路控制卡軟體負載平衡器 MUX 通訊

軟體負載平衡器多工器\(MUX\)和網路控制卡的通訊是透過 WCF 通訊協定，而使用憑證進行驗證。

根據預設，SCVMM 會挑選網路控制卡上設定的 SSL 憑證，並用於 southbound 通訊中的 Mux 裝置。 此憑證已在 「 NetworkControllerLoadBalancerMux"REST 資源和執行 Powershell cmdlet，即可檢視**Get NetworkControllerLoadBalancerMux**。

MUX REST 資源的範例\(部分\):

      "resourceId": "slbmux1.fabrikam.com",
      "properties": {
        "connections": [
          {
            "managementAddresses": [
               "slbmux1.fabrikam.com"
            ],
            "credential": {
              "resourceRef": "/credentials/a738762f-f727-43b5-9c50-cf82a70221fa"
            },
            "credentialType": "X509Certificate"
          }
        ],

進行相互驗證，您也必須將憑證上的 SLB MUX 裝置。 當您部署軟體負載平衡器使用 SCVMM 時，此憑證會自動設定 scvmm。

>[!IMPORTANT]
>在主機和 SLB 節點上，是嚴重的受信任的根憑證授權單位憑證存放區不包含任何憑證，「 發行至 」 不是 [發行者] 相同。 如果發生這種情況，網路控制站和 southbound 裝置之間的通訊將會失敗。

網路控制站和 SLB MUX 憑證必須受到彼此信任\(SLB MUX 憑證的根憑證必須位於受信任的根憑證授權單位儲存在網路控制站機器，反之亦然\)。 當您使用自我\-簽署憑證，SCVMM 可確保所需的憑證會出現在本機電腦存放區中受信任的根憑證授權單位。



