## <a name="standard-configuration-considerations"></a>標準組態考量

一律開啟 」 VPN 有許多組態選項。 不論您選擇您的 VPN 設定，不過，包含下列資訊：

-   **連接類型。** 連接通訊協定選取項目很重要，而且最終會與您將使用的驗證類型的相互關聯。 如需可用的通道通訊協定的詳細資訊，請參閱[VPN 連線類型](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-connection-type/)。

-   **路由。** 在此情況下，路由規則會判斷使用者是否可以使用其他的網路路由至 VPN 連線時。

    -   _分割通道_可讓您同時存取其他網路，例如網際網路。

    -   _強制通道_要求以獨佔方式透過 VPN 的所有流量，而且不允許同時存取其他網路。

-   **觸發。** _觸發_決定 （比方說，當裝置開啟時，手動使用者開啟應用程式），如何及何時會起始 VPN 連線。 觸發選項，請參閱[VPN 自動觸發型的設定檔選項](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-auto-trigger-profile/)。

-   **裝置或使用者驗證。** 一律開啟 」 VPN 會使用裝置憑證和裝置起始的連線，透過呼叫的功能[裝置通道](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config)。 該連線會自動起始，並為持續性，類似於 DirectAccess 基礎結構通道連線。

>[!TIP]
>當從 DirectAccess 移轉至一律開啟 」 VPN，請考慮開頭為相當於什麼，，然後從該處展開的組態選項。

藉由使用使用者憑證，一律開啟 」 VPN 用戶端自動連線，但它會在使用者層級 （使用者登入之後） 而非裝置層級 （使用者登入之前）。 體驗仍不讓使用者，但它支援更進階的驗證機制，例如 Windows hello 企業版。