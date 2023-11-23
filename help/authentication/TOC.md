---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 驗證
user-guide-description: Adobe Pass 驗證是 TV Everywhere 的權益解決方案，它提供模組化架構用來確定要求存取資源的人是否有權限存取該資源。
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Adobe Pass 驗證 說明 {#authentication}

+ [Adobe Pass驗證概觀](home.md)
+ Adobe Pass驗證概念 {#authentication-concepts}
   + [技術檔案](technical-paper.md)
   + [程式設計師概觀](programmer-overview.md)
   + [MVPD概覽](mvpd-overview.md)
+ Kickstart指南 {#kickstart-guides}
   + [程式設計師快速入門手冊](programmer-kickstart-guide.md)
   + [MVPD快速入門手冊](mvpd-kickstart-guide.md)
+ 程式設計師整合指南 {#programmer-integration-guide}
   + [程式設計師整合指南概觀](programmer-integration-guide-overview.md)
   + [程式設計師權益流程](entitlement-flow.md)
   + [程式設計師使用案例](programmer-use-cases.md)
   + [傳遞使用者端資訊（裝置、連線和應用程式）](passing-client-information-device-connection-and-application.md)
   + REST API {#restapi}
      + [REST API總覽](rest-api-overview.md)
      + [REST API逐步指南（伺服器對伺服器）](rest-api-cookbook-servertoserver.md)
      + [REST API逐步指南（使用者端對伺服器）](rest-api-cookbook-clienttoserver.md)
      + Rest API參考 {#rest-api-reference}
         + [REST API參考](rest-api-reference.md)
         + [註冊代碼要求](registration-code-request.md)
         + [傳回註冊記錄](return-registration-record.md)
         + [刪除註冊記錄](delete-registration-record.md)
         + [提供MVPD清單](provide-mvpd-list.md)
         + [啟動驗證](initiate-authentication.md)
         + [檢查驗證Token](check-authentication-token.md)
         + [擷取驗證Token](retrieve-authentication-token.md)
         + [啟動授權](initiate-authorization.md)
         + [擷取授權Token](retrieve-authorization-token.md)
         + [取得短媒體Token](obtain-short-media-token.md)
         + [依第二熒幕Web應用程式檢查驗證流程](check-authentication-flow-by-second-screen-web-app.md)
         + [擷取預先授權的資源清單](retrieve-list-of-preauthorized-resources.md)
         + [依第二熒幕Web應用程式擷取預先授權資源清單](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [啟動登出](initiate-logout.md)
         + [使用者中繼資料](user-metadata.md)
         + [擷取設定檔請求](retrieve-profilerequest.md)
         + [Token Exchange](token-exchange.md)
         + [臨時通票和促銷臨時通票的免費預覽](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + AccessEnabler SDK {#accessenabler-sdk}
      + JavaScript SDK {#javascriptsdk}
         + [JavaScript SDK總覽](javascript-sdk-overview.md)
         + [JavaScript SDK逐步指南](javascript-sdk-cookbook.md)
         + [JavaScript SDK API參考](javascript-sdk-api-reference.md)
         + 准則 {#js-sdk-guidelines}
            + [不需重新整理的登入和登出](refreshless-login-and-logout.md)
         + JavaScript API {#js-api}
            + [預先授權](js-preauthorize.md)
      + iOS/tvOS SDK {#ios-sdk}
         + [iOS/tvOS SDK總覽](iostvos-sdk-overview.md)
         + [iOS/tvOS SDK逐步指南](iostvos-sdk-cookbook.md)
         + [iOS/tvOS SDK API參考](iostvos-sdk-api-reference.md)
         + 准則 {#ios-tvos-sdk-guidelines}
            + [iOS/tvOS應用程式註冊](iostvos-application-registration.md)
            + 移轉准則 {#migration-guidelines}
               + [iOS/tvOS v3.x移轉指南](iostvos-v3x-migration-guide.md)
         + iOS/tvOS API {#ios-tvos-api}
            + [預先授權](preauthorize.md)
      + Android SDK {#androidsdk}
         + [Android SDK總覽](android-sdk-overview.md)
         + [Android SDK逐步指南](android-sdk-cookbook.md)
         + [Android SDK API參考](android-sdk-api-reference.md)
         + 准則 {#androidguidelines}
            + [Android應用程式註冊](android-application-registration.md)
            + [具有動態使用者端註冊的Android SDK](android-sdk-with-dynamic-client-registration.md)
         + Android API{#androidapi}
            + [預先授權](preauthorize-android.md)
      + Amazon FireOS SDK {#fireossdk}
         + [Amazon FireOS SSO — 程式設計師啟動指南](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [使用無使用者端API逐步指南的Amazon FireOS SSO](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Amazon FireOS技術概覽](amazon-fireos-technical-overview.md)
         + [Amazon FireOS整合逐步指南](amazon-fireos-integration-cookbook.md)
         + [Amazon FireOS API參考](amazon-fireos-native-client-api-reference.md)
         + [Amazon FireOS應用程式註冊](amazon-fireos-application-registration.md)
         + [具有動態使用者端註冊的FireOS SDK](fireos-sdk-with-dynamic-client-registration.md)
   + 平台SSO {#platform-sso}
      + APPLE SSO {#apple-sso}
         + [Apple SSO概觀](apple-sso-overview.md)
         + [Apple SSO逐步指南(REST API)](apple-sso-cookbook-rest-api.md)
         + [Apple SSO逐步指南(iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + Roku SSO {#roku-sso}
         + [Roku SSO](roku-sso-overview.md)
   + 內容中繼資料 {#content-metadata}
      + [識別受保護的資源](identify-protected-resources.md)
   + 內容伺服器整合 {#content-serv-int}
      + [如何整合媒體權杖驗證器](media-token-verifier-int.md)
   + 附錄 {#appendices}
      + [偵錯提示](appendix-b-debugging-tips.md)
+ MVPD整合指南 {#mvpd-int-guide}
   + [整合功能](mvpd-integr-features.md)
   + [驗證](authn-usecase.md)
   + [使用OAuth 2.0通訊協定進行驗證](authn-oauth2-protocol.md)
   + [Authorization](authz-usecase.md)
   + [預檢授權](mvpd-preflight-authz.md)
   + [mvpd登出](usecase-mvpd-logout.md)
   + [內容中繼資料交換](mvpd-content-metadata-exchange.md)
   + [使用者中繼資料交換](mvpd-user-metadata-exchng.md)
   + [Proxy MVPD Web服務](proxy-mvpd-webserv.md)
   + [Proxy MVPD SAML整合](proxy-mvpd-saml-int.md)
   + [服務提供者範圍](serv-provider-scoping.md)
   + [mvpd允許IP位址](mvpd-listing-ip-addres.md)
+ Adobe Pass驗證功能 {#auth-features}
   + Adobe Analytics整合 {#analytics-int}
      + [將Adobe Pass驗證伺服器端資料整合至Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [在Adobe Pass驗證中使用Experience CloudID](exp-cloud-id-authn.md)
   + 軟體權利檔案服務監視 {#entitlement-service-monitoring}
      + [軟體權利檔案服務監視概觀](entitlement-service-monitoring-overview.md)
      + [權益服務監視API](entitlement-service-monitoring-api.md)
      + [伺服器端量度](understanding-serverside-metrics.md)
   + 暫時通過 {#temp-pass}
      + [暫時通過](temp-pass.md)
      + [促銷臨時傳遞](promotional-temp-pass.md)
      + [重設暫時通過](reset-temp-pass.md)
   + 單一登入 {#sso}
      + [單一登入支援](sso-support.md)
      + [透過被動驗證的SSO](sso-passive-authn.md)
   + 以住家為基礎的驗證 {#home-based-auth}
      + [適用於所有地方電視的家庭式驗證](home-based-authn-tve.md)
      + [MVPD的HBA狀態](hba-status-mvpds.md)
   + 使用者中繼資料 {#user-metadat}
      + [使用者中繼資料](user-metadata-feature.md)
   + [預檢授權](preflight-authz.md)
   + 錯誤報告 {#error-reportn}
      + [錯誤報告](error-reporting.md)
      + [增強的錯誤碼](enhanced-error-codes.md)
   + 使用者端註冊 {#client-regn}
      + [動態使用者端註冊](dynamic-client-registration.md)
      + [動態使用者端註冊API](dynamic-client-registration-api.md)
      + [動態使用者端註冊管理](dynamic-client-registration-management.md)
   + 降級服務 {#degrn-service}
      + [降級API概觀](degradation-api-overview.md)
   + 隱私權整備 {#privacy-readiness}
      + [隱私權支援概述](privacy-supp-overview.md)
      + [如何提出隱私權請求](make-privacy-req.md)
+ 提示與疑難排解 {#tips-troubleshoot}
   + [在選取對話方塊中允許MVPD](allow-mvpd-selectn-dialog.md)
   + [防止MVPD出現在選取對話方塊中](prevent-mvpd-selectn-dialog.md)
+ 支援 {#support}
   + [向上呈報程式](escalation-procedures.md)
   + [監控Adobe PassAdobePayTV Pass](monitoring-adobe-pay-tv-pass.md)
   + [最低系統需求](minimum-system-requirements.md)
+ 發行說明 {#release-notes}
   + [Adobe Pass Authentication 2.68發行說明](auth-rn-268.md)
   + [Adobe Pass Authentication 2.67發行說明](auth-rn-267.md)
   + [Adobe Pass Authentication 2.66發行說明](auth-rn-266.md)
   + [Adobe Pass Authentication 2.65.1發行說明](auth-rn-2651.md)
   + [Adobe Pass Authentication 2.65發行說明](auth-rn-265.md)
   + [Adobe Pass Authentication 2.64.1發行說明](auth-rn-2641.md)
   + [Adobe Pass Authentication 2.64發行說明](auth-rn-264.md)
   + [Adobe Pass Authentication 2.63發行說明](auth-rn-263.md)
   + [Adobe Pass Authentication 2.62.1發行說明](auth-rn-2621.md)
   + iOS/tvOS SDK發行說明  {#release-notes-ios}
      + [Adobe Pass Authentication iOS / tvOS 3.8.2發行說明](authn-rn-ios-tvos-382.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.1發行說明](authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication iOS / tvOS 3.7.0發行說明](authn-rn-ios-tvos-370.md)
   + Android SDK發行說明 {#release-notes-android}
      + [Adobe Pass Authentication Android 3.7.3發行說明](authn-rn-android-373.md)
+ 技術注意事項 {#tech-notes}
   + Adobe Pass驗證SDK {#primetime-authentication-sdks}
      + [憑證問答](certificates-qa.md)
      + JavaScript SDK {#javascript}
         + [Safari瀏覽器的JS SDK限制](js-sdk-limitations-for-safari-browser.md)
         + [Cookie更新 — SameSite和Secure標幟](cookies-updates--samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Android 10應用程式上的Access Enabler Android SDK單一登入(SSO)](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass驗證和Android 6「Marshmallow」新許可權模型](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS SDK {#iostvos}
         + [iOS SDK 3.1+上的WKWebView支援](wkwebview-support-on-ios-sdk-31.md)
         + [iOS SDK 3.2+上的SFSafariViewController支援](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [使用iOS Authentication Access Enabler時Adobe Pass上的SSO](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS驗證錯誤 — 找不到adobepass.ios.app](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [在iOS上重設暫時傳遞](reset-temp-pass-on-ios.md)
         + [使用主控台應用程式記錄檔對AccessEnabler iOS/tvOS SDK進行除錯](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [AccessEnabler iOS/tvOS 3.7.0升級路徑](accessenabler-iostvos-370-upgrade-path.md)
   + 通過驗證環境 {#primetime-authentication-environments}
      + [瞭解Adobe環境](understanding-the-adobe-environments.md)
      + [在預備中設定您的環境及測試](setting-up-your-environment-and-testing-in-prequal.md)
      + [如何使用Adobe API測試網站測試驗證和授權流程](test-authn-authz-flows-using-adobes-api-test-site.md)
   + 無使用者端API {#clientless-api}
      + [無使用者端API實施 — 錯誤代碼/包含可能原因/原因的訊息](clientless-api-implementation-error-codes--messages-with-probable-reason--cause.md)
      + [缺少裝置ID時的無使用者端API流程](clientless-api-flow-in-the-absence-of-device-id.md)
      + [無使用者端：避免在/authenticate請求中使用&#39;&amp;&#39;reg_code](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [在Xbox 360和XboxOne無使用者端上啟用程式設計師的Adobe Pass軟體權利檔案服務](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [無使用者端裝置型別和量度](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + 使用者體驗 {#user-exp}
      + [如何將MVPD登入頁面從iFrame移轉至快顯視窗](migr-mvpd-login-iframe-popup.md)
      + [預檢功能：如何啟用、疑難排解或判斷問題](preflight-feature.md)
   + 工具與公用程式 {#tools-and-utilities}
      + [使用Charles代理](using-charles-proxy.md)
   + 概念 {#concepts}
      + [瞭解使用者ID](understanding-user-ids.md)
+ [TVE儀表板使用手冊](tve-dashboard-user-guide.md)
+ [字彙表](glossary.md)
