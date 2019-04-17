---
title: 管理憑證的軟體定義網路
description: 若要了解如何管理 Northbound 網路控制器和 Southbound 通訊的憑證部署軟體定義網路 (SDN) 在 Windows Server 2016 Datacenter 時，您可以使用此主題。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: c4e2f6c7-0364-4bf8-bb66-9af59c0bbd74
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3036de9161cabc2f3a85a1d3b2ce7739f0ff6bd3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-for-software-defined-networking"></a>管理憑證的軟體定義網路

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要了解如何管理 Northbound 網路控制器和 Southbound 通訊的憑證在 Windows Server 2016 Datacenter 的軟體定義網路 \(SDN\) 部署您正在使用 System Center 一樣 Manager \(SCVMM\) 為您 SDN 管理 client 時，您可以使用此主題。

>[!NOTE]
>Network Controller 概觀資訊，請查看[Network Controller](../technologies/network-controller/Network-Controller.md)。

如果您不使用 Kerberos 保護的網路控制器通訊，您可以使用 x.509 驗證、 授權及加密。

在 Windows Server 2016 Datacenter SDN 支援兩 self\ 簽章和憑證授權單位 \ (CA\)-簽署 x.509。 本主題提供逐步指示建立這些憑證，並將其套用的安全的網路控制器 Northbound 的通訊通道與管理戶端和 Southbound 通訊網路的裝置，例如軟體負載平衡器 \(SLB\)。
.
當您使用 certificate\ 驗證時，您必須註冊 Network Controller 節點上一會以下列方式使用的憑證。

1. 加密 Northbound 安全通訊端層 \(SSL\) Network Controller 節點和管理戶端，例如 System Center 一樣 Manager 間通訊。
2. Network Controller 節點和 Southbound 裝置之間的服務，例如 HYPER-V 主機和軟體負載平衡器 \(SLBs\) 驗證。

## <a name="creating-and-enrolling-an-x509-certificate"></a>建立和註冊 X.509

您可以建立和註冊 self\ 簽署的憑證或 CA 發行憑證。

>[!NOTE]
>當您使用 SCVMM 部署 Network Controller 時，您必須指定 X.509 憑證所使用的網路控制器服務範本設定期間加密 Northbound 通訊。

憑證設定必須包含下列值。

- 值為**RestEndPoint**文字方塊必須的網路控制器完整網域名稱 \(FQDN\) 或 IP 位址。 
- **RestEndPoint**值必須符合主體名稱 \ （一般的名稱、 CN\） X.509 憑證。

### <a name="creating-a-self-signed-x509-certificate"></a>建立 Self\ 簽署 X.509 憑證

您可以建立自我的 X.509 憑證，並且私密金鑰與匯出 \ （受 password\） 依照下列步驟針對 single\ 節點和 multiple\ 節點 Network Controller 的部署。

當您建立 self\ 簽署的憑證時，您可以使用下列指導方針。

- 您可以使用控制器其餘端點網路的 IP 位址 DnsName 參數-，但不是建議，因為您必須 Network Controller 節點是一個管理子網路中都位於 \ （例如在單一 rack\)
- 對於多個節點 NC 部署，指定 DNS 名稱將會變成的網路控制器叢集 FQDN \ (DNS 主機 A 記錄會自動建立。 \) 
- 單一節點 Network Controller 部署的 DNS 名稱可能 Network Controller 的主機名稱緊接著的完整網域名稱。

#### <a name="multiple-node"></a>多個節點

您可以使用[新-SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令來建立 self\ 簽署的憑證。

**語法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCRESTName>")

**使用範例**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "MultiNodeNC" -DnsName @("NCCluster.Contoso.com")

#### <a name="single-node"></a>單一節點

您可以使用[新-SelfSignedCertificate](https://technet.microsoft.com/en-us/itpro/powershell/windows/pkiclient/new-selfsignedcertificate) Windows PowerShell 命令來建立 self\ 簽署的憑證。

**語法**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "<YourNCComputerName>" -DnsName @("<NCFQDN>")

**使用範例**

    New-SelfSignedCertificate -KeyUsageProperty All -Provider "Microsoft Strong Cryptographic Provider" -FriendlyName "SingleNodeNC" -DnsName @("SingleNodeNC.Contoso.com")

### <a name="creating-a-ca-signed-x509-certificate"></a>建立 CA\ 簽署 X.509 憑證

若要使用 CA 建立憑證，您必須已經部署 Active Directory 憑證服務 \(AD CS\) 公用基礎結構的 \(PKI\)。 

>[!NOTE]
>您可以使用第三方 Ca 或工具，例如 openssl，以建立網路控制器，請使用的憑證不過特定 AD CS 本主題中的指示進行。 了解如何使用第三方 CA 或工具，以查看您正在使用的軟體的文件。

建立 CA 憑證包含下列步驟。

1. 您組織的網域或安全性系統管理員可以設定憑證範本
2. 您組織的 Controller 的網路管理員或 SCVMM 系統管理員會從 CA 要求一個新的憑證。

#### <a name="certificate-configuration-requirements"></a>憑證設定需求

當您設定的憑證範本下一個步驟中時，請確定您所設定的範本包含下列所需的項目。

1. 必須 HYPER-V 主機的 FQDN 憑證主體名稱。
2. 憑證必須放在個人本機存放區 (我 – 憑證： \localmachine\my)
3. 憑證必須有兩個伺服器的驗證 (EKU: 1.3.6.1.5.5.7.3.1) 和 Client 驗證 (EKU: 1.3.6.1.5.5.7.3.2) 應用程式原則。

>[!NOTE]
>如果個人 \ (我 – 憑證：\localmachine\my\) 憑證存放區 Hyper\ HYPER-V 主機上的有一個以上 X.509 主機完整網域名稱 \(FQDN\) 憑證的主體名稱 (DATA-CN)，請確定將會使用透過 SDN 憑證已 OID 1.3.6.1.4.1.311.95.1.1.1 與其他自訂增強鍵使用量屬性。 否則，網路控制器和主機間通訊可能無法運作。

#### <a name="to-configure-the-certificate-template"></a>若要設定憑證範本
  
>[!NOTE]
>您可以執行此程序之前，您應該檢視憑證需求和可用的憑證範本 \ [憑證範本主控台中。 您可以修改現有的範本或建立重複的現有的範本，然後修改您複製的範本。 建立現有範本一份建議。

1. 在 [伺服器 AD CS 安裝的位置，在伺服器管理員中，按一下**工具**，然後按一下 [**憑證授權單位**。 憑證授權單位 Microsoft Management Console \(MMC\) 開啟。 
2. 在 MMC 中，按兩下 [CA 名稱，以滑鼠右鍵按一下**憑證範本**，然後按**管理**。
3. [憑證範本主控台開啟。 詳細資料窗格中會顯示所有的憑證範本。
4. 在詳細資料窗格中，按一下您要複製範本。
5.  按一下**動作**，然後再按**複製範本**。 範本**屬性**對話方塊。
6.  在範本**屬性**對話方塊中，於**主體名稱**索引標籤上，按一下 [**中要求提供**。 \ (此設定是必要的網路控制器 SSL 憑證。 \)
7.  在範本**屬性**對話方塊中，於**處理要求**索引標籤時，請確定**允許私密金鑰匯出**選取。 也確保**簽章和加密**選取用途。
8.  範本中**屬性**對話方塊中，於**擴充功能**索引標籤，選取**鍵使用**，然後按一下**編輯**。
9.  在**簽章**，確認**數位簽章**選取。
10.  範本中**屬性**對話方塊中，於**的擴充功能**索引標籤，選取**應用程式原則**，然後按一下**編輯**。
11.  在**應用程式原則**，確保**Client 驗證**和**伺服器驗證**優先順序。
12.  將儲存複本憑證範本唯一名稱，例如**Network Controller 範本**。

#### <a name="to-request-a-certificate-from-the-ca"></a>若要從 CA 憑證

您可以使用的憑證嵌入式管理單元要求憑證。 您可以要求任何預先設定並使用由系統管理員的身分處理憑證要求的 ca 憑證的類型。

**使用者**或**系統管理員**，才能完成此程序最小群組成員資格。

1. 打開憑證嵌入式管理單元電腦。
2. 主控台中，按一下 [**的憑證 \(Local Computer\)**。 選取 [**個人**憑證存放區。
3. 在**動作**功能表，指向 [* * 所有工作 * *，然後再按一下**要求新的憑證**以開始憑證註冊精靈。 按一下**下一步**。
4. 選取 [**設定您的系統管理員的**憑證註冊原則和**下**。
5. 選取 [ **Active Directory 註冊原則**\ （根據您設定在上一個 section\ CA 範本）。
6. 展開**的詳細資料**區段，設定下列項目。
    1. 確保**鍵使用量**包含兩 * * 數位簽章 * * 和**鍵加密**。
    2. 確認**的應用程式原則**兩者都包含**伺服器驗證**\(1.3.6.1.5.5.7.3.1\) 和**Client 驗證**\(1.3.6.1.5.5.7.3.2\)。
7. 按一下**屬性**。
8. 在**主旨**索引標籤的**主體名稱**，請在**輸入**，選取**一般名稱**。 在 [值指定**網路控制器其餘端點**。
9. 按一下**套用**，然後按**[確定]**。
10. 按一下**註冊**。

在憑證 MMC 中，按一下個人檢視您擁有退出從 CA 憑證存放區。

## <a name="exporting-and-copying-the-certificate-to-the-scvmm-library"></a>匯出和憑證複製到 SCVMM 媒體櫃

建立 self\ 簽署或 CA\ 簽署的憑證之後, 您必須使用私密金鑰匯出憑證 \(in.pfx format\) 私密金鑰不 \ （在 64 基本.cer format\) 從 「 憑證嵌入式管理單元。 

您必須再兩個匯出將檔案複製到**ServerCertificate.cr**和**NCCertificate.cr**資料夾，指定您匯入 NC 服務範本時間。

1. 打開憑證嵌入式管理單元 (certlm.msc)，並找出的憑證在本機電腦的個人憑證存放區。
2. Right\ 按一下憑證，按一下 [**的所有任務**，然後按一下 [**匯出**。 憑證匯出精靈開啟。 按一下**下一步**。
3. 選取 [ **[是]**匯出私密金鑰] 選項，按**下一步**。
4. 選擇 [**個人資訊交換-PKCS #12 (。PFX)** ，並接受預設為**包含所有的憑證憑證路徑中**若有可能。
5. 指派使用者或群組和憑證您要匯出的密碼，請按一下**下一步**。
6. 若要匯出頁檔案，瀏覽您想要放置匯出的檔案的位置，它命名。
7. 同樣地，匯出中的憑證。CER 格式。 注意： 若要匯出。CER 格式，取消選取 [是，匯出私密金鑰] 選項。
8. 複製。PFX ServerCertificate.cr 資料夾。
9. 複製。CER NCCertificate.cr 資料夾的檔案。

當您完成後時，請重新整理 SCVMM 媒體櫃中的這些資料夾，並確定您有複製這些憑證。 請繼續進行網路控制器服務範本設定和部署。

## <a name="authenticating-southbound-devices-and-services"></a>驗證 Southbound 裝置與服務 

網路控制器通訊主機和 SLB MUX 裝置使用驗證憑證。 通訊的主機是 OVSDB 通訊協定與 SLB MUX 裝置通訊時 WCF 通訊協定。

### <a name="hyper-v-host-communication-with-network-controller"></a>Network Controller 的主機 HYPER-V 通訊

透過 OVSDB HYPER-V 主機的通訊，Network Controller 需要呈現主機上的憑證。 根據預設，SCVMM 挑選設定網路控制器上的 SSL 憑證，並使用它來進行 southbound 通訊的主機。

這就是為何 SSL 憑證必須 Client 驗證 EKU 設定的原因。 此憑證已設定 「 伺服器 」 的其餘資源 \ （HYPER-V 主機以在 Network Controller 伺服器 resource\），並可執行 Windows PowerShell 命令**取得-NetworkControllerServer**。

以下是範例部分的伺服器的其餘部分資源。

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

對於互加好友的驗證，HYPER-V 主機也必須通訊 Network Controller 的憑證。 

您可以註冊憑證授權單位 \(CA\) 的憑證。 如果您在主機上找不到根據 CA 憑證，SCVMM 建立自動簽署的憑證，並 provisions 該主機上。

網路控制器和 HYPER-V 主機彼此必須信任的憑證。 必須位於 HYPER-V 主機憑證根憑證網路控制器受信任的根憑證授權單位儲存在本機電腦，，反之亦然。 

當您正在使用 self\ 簽署的憑證時，可確保 SCVMM 必要的憑證的本機電腦的受信任的根憑證授權單位存放區中。 

如果您使用根據 CA 憑證 HYPER-V 主機，您需要確保根憑證會出現 Network Controller 的受信任的根憑證授權單位網上商店的本機電腦上。

### <a name="software-load-balancer-mux-communication-with-network-controller"></a>軟體與 Network Controller 負載平衡器 MUX 通訊

軟體負載平衡器 Multiplexor \(MUX\) 和 Network Controller 通訊 WCF 通訊協定進行驗證使用的憑證。

根據預設，SCVMM 挑選 SSL 憑證網路控制器上的設定，以及使用它的 Mux 裝置 southbound 通訊。 在 「 NetworkControllerLoadBalancerMux 」 上設定此憑證將資源，並可透過執行 Powershell cmdlet**取得-NetworkControllerLoadBalancerMux**。

其他 MUX 資源 \(partial\) 的範例：

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

對於互加好友的驗證，您必須同時憑證 SLB MUX 的裝置上。 部署使用 SCVMM 軟體負載平衡器時，此憑證會自動設定來 SCVMM。

>[!IMPORTANT]
>主機和 SLB 節點，很重要的受信任的根憑證授權單位憑證存放區中不包含任何憑證其中，「 發行至 「 不是 「 發行者 」 一樣。 發生這種情形，如果 Network Controller and southbound 裝置間通訊失敗。

Network Controller 及 SLB MUX 憑證必須信任彼此 \ （SLB MUX 憑證的根憑證必須在網路控制器電腦受信任的根憑證授權單位儲存和虎鉗 versa\）。 當您正在使用 self\ 簽署的憑證時，可確保 SCVMM 必要的憑證會出現在受信任的根憑證授權單位 」 中儲存的本機電腦。



