---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 驗證
user-guide-description: Adobe Pass 驗證是 TV Everywhere 的權益解決方案，它提供模組化架構用來確定要求存取資源的人是否有權限存取該資源。
source-git-commit: acff285f7db1bdd32d5da3e01a770d9581d3ba75
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 2%

---


# Adobe Pass驗證說明 {#authentication}

+ [Adobe Pass驗證概觀](home.md)
+ Adobe Pass驗證概念{#authentication-concepts}
   + [技術檔案](technical-paper.md)
   + [程式設計師概觀](programmer-overview.md)
   + [MVPD概覽](mvpd-overview.md)
+ Kickstart指南{#kickstart-guides}
   + [程式設計師快速入門手冊](programmer-kickstart-guide.md)
   + [MVPD快速入門手冊](mvpd-kickstart-guide.md)
+ 程式設計師整合指南{#programmer-integration-guide}
   + [程式設計師整合指南概觀](programmer-integration-guide-overview.md)
   + [程式設計師權益流程](entitlement-flow.md)
   + [程式設計師使用案例](programmer-use-cases.md)
   + [傳遞使用者端資訊（裝置、連線和應用程式）](passing-client-information-device-connection-and-application.md)
   + [節流機制](throttling-mechanism.md)
   + REST API V1 {#rest-api-v1}
      + [REST API總覽](rest-api-overview.md)
      + [REST API逐步指南（伺服器對伺服器）](rest-api-cookbook-servertoserver.md)
      + [REST API逐步指南（使用者端對伺服器）](rest-api-cookbook-clienttoserver.md)
      + Rest API參考{#rest-api-reference}
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
   + REST API V2 {#rest-api-v2}
      + API {#rest-api-v2-apis}
         + [REST API V2 - API — 概觀](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
         + 設定{#rest-api-v2-configuration-apis}
            + [擷取特定服務提供者的設定](./rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
         + 工作階段{#rest-api-v2-sessions-apis}
            + [建立驗證工作階段](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
            + [繼續驗證工作階段](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
            + [擷取驗證工作階段](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
            + [在使用者代理程式中執行驗證](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
         + 設定檔{#rest-api-v2-profiles-apis}
            + [擷取設定檔](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
            + [擷取特定mvpd的設定檔](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
            + [擷取特定程式碼的設定檔](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
         + 決定{#rest-api-v2-decisions-apis}
            + [使用特定mvpd擷取授權決策](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
            + [使用特定mvpd擷取預先授權決定](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
         + 登出{#rest-api-v2-logout-apis}
            + [啟動特定mvpd的登出](./rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
         + 合作夥伴單一登入{#rest-api-v2-partner-single-sign-on-apis}
            + [擷取合作夥伴驗證請求](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
            + [使用合作夥伴驗證回應擷取設定檔](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
      + 流程{#rest-api-v2-flows}
         + [REST API V2 — 流程 — 概觀](./rest-api-v2/flows/rest-api-v2-flows-overview.md)
         + 基本存取流程{#rest-api-v2-basic-access-flows}
            + [主要應用程式內執行的基本設定檔流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
            + [在次要應用程式內執行的基本設定檔流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
            + [主要應用程式內執行的基本驗證流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
            + [在次要應用程式內執行的基本驗證流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
            + [主要應用程式內執行的基本授權流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
            + [主要應用程式內執行的基本預先授權流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
            + [主要應用程式內執行的基本登出流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
         + 降級存取流程{#rest-api-v2-degraded-access-flows}
            + [存取流程效能降低](rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
         + 暫時存取流程{#rest-api-v2-temporary-access-flows}
            + [暫時存取流程](rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
         + 單一登入存取流程{#rest-api-v2-single-sign-on-access-flows}
            + [使用合作夥伴流程的單一登入](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
            + [使用平台身分流程的單一登入](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
            + [使用服務權杖流程的單一登入](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
            + [單一登出流程](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
      + 附錄{#rest-api-v2-appendix}
         + 標頭{#rest-api-v2-appendix-headers}
            + [頁首 — 授權](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
            + [頁首 — AP-Device-Identifier](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
            + [頁首 — X-Device-Info](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
            + [標頭 — AD-Service-Token](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
            + [標題 — Adobe-Subject-Token](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
            + [頁首 — AP-Partner-Framework-Status](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
            + [頁首 — AP-TempPass-Identity](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + AccessEnabler SDK {#accessenabler-sdk}
      + JavaScript SDK {#javascriptsdk}
         + [JavaScript SDK概觀](javascript-sdk-overview.md)
         + [JavaScript SDK逐步指南](javascript-sdk-cookbook.md)
         + [JavaScript SDK API參考](javascript-sdk-api-reference.md)
         + 准則{#js-sdk-guidelines}
            + [不需重新整理的登入和登出](refreshless-login-and-logout.md)
         + JavaScript API {#js-api}
            + [預先授權](js-preauthorize.md)
      + iOS/tvOS SDK {#ios-sdk}
         + [iOS/tvOS SDK總覽](iostvos-sdk-overview.md)
         + [iOS/tvOS SDK逐步指南](iostvos-sdk-cookbook.md)
         + [iOS/tvOS SDK API參考](iostvos-sdk-api-reference.md)
         + 准則{#ios-tvos-sdk-guidelines}
            + [iOS/tvOS應用程式註冊](iostvos-application-registration.md)
            + 移轉准則{#migration-guidelines}
               + [iOS/tvOS v3.x移轉指南](iostvos-v3x-migration-guide.md)
            + [iOS/tvOS儲存完整性檢查](iostvos-sdk-storage-integrity-checks.md)
         + iOS/tvOS API {#ios-tvos-api}
            + [預先授權](preauthorize.md)
      + Android SDK {#androidsdk}
         + [Android SDK概觀](android-sdk-overview.md)
         + [Android SDK逐步指南](android-sdk-cookbook.md)
         + [Android SDK API參考](android-sdk-api-reference.md)
         + 准則{#androidguidelines}
            + [Android應用程式註冊](android-application-registration.md)
            + [具Dynamic Client註冊的Android SDK](android-sdk-with-dynamic-client-registration.md)
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
      + Apple SSO {#apple-sso}
         + [Apple SSO概觀](apple-sso-overview.md)
         + [Apple SSO逐步指南(REST API)](apple-sso-cookbook-rest-api.md)
         + [Apple SSO逐步指南(iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + Roku SSO {#roku-sso}
         + [Roku SSO](roku-sso-overview.md)
   + 內容中繼資料{#content-metadata}
      + [識別受保護的資源](identify-protected-resources.md)
   + 內容伺服器整合{#content-serv-int}
      + [如何整合媒體權杖驗證器](media-token-verifier-int.md)
   + 附錄{#appendices}
      + [偵錯提示](appendix-b-debugging-tips.md)
+ MVPD整合指南{#mvpd-int-guide}
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
+ Adobe Pass驗證功能{#auth-features}
   + Adobe Analytics整合{#analytics-int}
      + [將Adobe Pass驗證伺服器端資料整合至Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [在Adobe Pass驗證中使用Experience CloudID](exp-cloud-id-authn.md)
   + 權益服務監視{#entitlement-service-monitoring}
      + [軟體權利檔案服務監視概觀](entitlement-service-monitoring-overview.md)
      + [權益服務監視API](entitlement-service-monitoring-api.md)
      + [伺服器端量度](understanding-serverside-metrics.md)
   + 暫時傳遞{#temp-pass}
      + [暫時通過](temp-pass.md)
      + [促銷臨時傳遞](promotional-temp-pass.md)
      + [重設暫時通過](reset-temp-pass.md)
   + 單一登入{#sso}
      + [單一登入支援](sso-support.md)
      + [透過被動驗證的SSO](sso-passive-authn.md)
   + 以住家為基礎的驗證{#home-based-auth}
      + [適用於所有地方電視的家庭式驗證](home-based-authn-tve.md)
      + [MVPD的HBA狀態](hba-status-mvpds.md)
   + 使用者中繼資料{#user-metadat}
      + [使用者中繼資料](user-metadata-feature.md)
   + [預檢授權](preflight-authz.md)
   + 報告{#error-reportn}時發生錯誤
      + [錯誤報告](error-reporting.md)
      + [增強的錯誤碼](enhanced-error-codes.md)
   + 使用者端註冊{#dcr-api}
      + [動態使用者端註冊概觀](./dcr-api/dynamic-client-registration-overview.md)
      + API {#dcr-api-apis}
         + [擷取使用者端認證](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
         + [擷取存取權杖](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
      + 流程{#dcr-api-flows}
         + [動態使用者端註冊流程](./dcr-api/flows/dynamic-client-registration-flow.md)
   + 降級服務{#degrn-service}
      + [降級API概觀](degradation-api-overview.md)
   + 隱私權準備{#privacy-readiness}
      + [隱私權支援概述](privacy-supp-overview.md)
      + [如何提出隱私權請求](make-privacy-req.md)
+ 提示與疑難排解{#tips-troubleshoot}
   + [在選取對話方塊中允許MVPD](allow-mvpd-selectn-dialog.md)
   + [防止MVPD出現在選取對話方塊中](prevent-mvpd-selectn-dialog.md)
+ 支援{#support}
   + [向上呈報程式](escalation-procedures.md)
   + [監控Adobe PassAdobePayTV Pass](monitoring-adobe-pay-tv-pass.md)
   + [最低系統需求](minimum-system-requirements.md)
+ 發行說明{#release-notes}
   + [Adobe Pass Authentication 3.0發行說明](auth-rn-300.md)
   + [Adobe Pass Authentication 2.70發行說明](auth-rn-270.md)
   + [Adobe Pass Authentication 2.69發行說明](auth-rn-269.md)
   + [Adobe Pass Authentication 2.68發行說明](auth-rn-268.md)
   + [Adobe Pass Authentication 2.67發行說明](auth-rn-267.md)
   + [Adobe Pass Authentication 2.66發行說明](auth-rn-266.md)
   + [Adobe Pass Authentication 2.65.1發行說明](auth-rn-2651.md)
   + [Adobe Pass Authentication 2.65發行說明](auth-rn-265.md)
   + [Adobe Pass Authentication 2.64.1發行說明](auth-rn-2641.md)
   + [Adobe Pass Authentication 2.64發行說明](auth-rn-264.md)
   + [Adobe Pass Authentication 2.63發行說明](auth-rn-263.md)
   + [Adobe Pass Authentication 2.62.1發行說明](auth-rn-2621.md)
   + JavaScript SDK發行說明{#release-notes-javascript}
      + [Adobe Pass Authentication JavaScript 4.7.0發行說明](authn-rn-javascript-470.md)
      + [Adobe Pass Authentication JavaScript 4.6.0發行說明](authn-rn-javascript-460.md)
      + [Adobe Pass Authentication JavaScript 4.4.0發行說明](authn-rn-javascript-440.md)
      + [Adobe Pass Authentication JavaScript 4.2.0發行說明](authn-rn-javascript-420.md)
      + [Adobe Pass Authentication JavaScript 4.1.1發行說明](authn-rn-javascript-411.md)
      + [Adobe Pass Authentication JavaScript 4.1.0發行說明](authn-rn-javascript-410.md)
      + [Adobe Pass Authentication JavaScript 4.0.0發行說明](authn-rn-javascript-400.md)
      + [Adobe Pass Authentication JavaScript 3.5.0發行說明](authn-rn-javascript-350.md)
   + iOS/tvOS SDK發行說明{#release-notes-ios}
      + [Adobe Pass Authentication iOS / tvOS 3.9.2發行說明](authn-rn-ios-tvos-392.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.4發行說明](authn-rn-ios-tvos-384.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.3發行說明](authn-rn-ios-tvos-383.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.2發行說明](authn-rn-ios-tvos-382.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.1發行說明](authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication iOS / tvOS 3.7.0發行說明](authn-rn-ios-tvos-370.md)
   + Android SDK發行說明{#release-notes-android}
      + [Adobe Pass Authentication Android 3.7.3發行說明](authn-rn-android-373.md)
+ 技術說明{#tech-notes}
   + Adobe Pass驗證SDK {#primetime-authentication-sdks}
      + [憑證問答](certificates-qa.md)
      + JavaScript SDK {#javascript}
         + [追蹤預防評估 — Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [預防追蹤評估 — Google Chrome](tracking-prevention-assessment-google-chrome.md)
         + [Cookie更新 — SameSite和Secure標幟](cookies-updates-samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Android 10應用程式上的Access Enabler Android SDK單一登入(SSO)](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass驗證和Android 6 「Marshmallow」新許可權模型](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS SDK {#iostvos}
         + [iOS SDK 3.1+上的WKWebView支援](wkwebview-support-on-ios-sdk-31.md)
         + [iOS SDK 3.2+上的SFSafariViewController支援](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [使用iOS Authentication Access Enabler時Adobe Pass上的SSO](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS驗證錯誤 — 找不到adobepass.ios.app](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [在iOS上重設暫時傳遞](reset-temp-pass-on-ios.md)
         + [使用主控台應用程式記錄檔對AccessEnabler iOS/tvOS SDK進行除錯](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [AccessEnabler iOS/tvOS 3.7.0升級路徑](accessenabler-iostvos-370-upgrade-path.md)
   + 通過驗證環境{#primetime-authentication-environments}
      + [瞭解Adobe環境](understanding-the-adobe-environments.md)
      + [在預備中設定您的環境及測試](setting-up-your-environment-and-testing-in-prequal.md)
      + [如何使用Adobe API測試網站測試驗證和授權流程](test-authn-authz-flows-using-adobes-api-test-site.md)
   + 無使用者端API {#clientless-api}
      + [無使用者端API實施 — 錯誤代碼/包含可能原因/原因的訊息](clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [缺少裝置ID時的無使用者端API流程](clientless-api-flow-in-the-absence-of-device-id.md)
      + [無使用者端：避免在/authenticate請求中使用&#39;&amp;&#39;reg_code](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [在Xbox 360和XboxOne無使用者端上啟用程式設計師的Adobe Pass軟體權利檔案服務](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [無使用者端裝置型別和量度](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + 使用者體驗{#user-exp}
      + [如何將MVPD登入頁面從iFrame移轉至快顯視窗](migr-mvpd-login-iframe-popup.md)
      + [預檢功能：如何啟用、疑難排解或判斷問題](preflight-feature.md)
   + 工具與公用程式{#tools-and-utilities}
      + [使用Charles代理](using-charles-proxy.md)
   + 概念{#concepts}
      + [瞭解使用者ID](understanding-user-ids.md)
+ [TVE儀表板使用手冊](tve-dashboard/old-tve-dashboard/tve-dashboard-user-guide.md)
+ 新TVE儀表板使用手冊{#user-guide}
   + [TVE儀表板概觀](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md)
   + [環境](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-environments.md)
   + [檢閱和推送變更](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [儀表板](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-home.md)
   + [頻道](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md)
   + [程式設計師](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPDs](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-mvpds.md)
   + [整合](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md)
   + [報表](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-reports.md)
   + [變更記錄](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-changes-log.md)
+ [字彙表](glossary.md)
