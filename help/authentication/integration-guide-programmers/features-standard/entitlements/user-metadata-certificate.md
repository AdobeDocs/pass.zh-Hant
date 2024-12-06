---
title: 用於加密的使用者中繼資料憑證
description: 用於加密的使用者中繼資料憑證
exl-id: 6f5d9a31-945e-418b-a9df-985bdbf29dff
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# 用於加密的使用者中繼資料憑證

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

針對加密的使用者中繼資料Adobe Pass驗證整合，您需要有私人/公開金鑰組。

本檔案說明產生公開金鑰憑證以用於Adobe Pass驗證的程式。 此處說明的程式使用OpenSSL工具組。

## 憑證產生程式逐步說明(#generation)

1. 下載並安裝OpenSSL工具組(http://www.openssl.org)。

1. 產生憑證申請檔(CSR)：

   * 產生金鑰組。  開啟「命令/終端機」視窗，然後執行下列命令：

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * 產生CSR。 在命令列上，執行下列動作：

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     系統會提示您輸入私密金鑰的密碼。

   * 建立您的私密金鑰和密碼的備份復本。 範例CSR：

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. 將CSR傳送給憑證授權單位(CA) （例如Verisign）。

1. CA會以.p7b格式（PKCS#7、密碼編譯訊息語法標準）將憑證傳送給您

1. 部署.p7b憑證。 使用私密金鑰將PKCS#7 (.p7b)檔案轉換為PKCS#12 （PFX檔案、個人資訊交換語法標準），並產生PEM檔案（串連的憑證容器檔案）：

   * 將PKCS#7檔案轉換為暫存PEM檔案。 在命令列上，執行下列動作：

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * 將暫存PEM檔案轉換為PFX檔案。  在命令列上，執行下列動作：

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * 將暫存PEM檔案轉換為最終PEM檔案。 在命令列上，執行下列動作：

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. 將最終PEM檔案傳送給Adobe進行設定。

   * 最終需要接收PEM檔案的人是指派給您的整合/驗證的Adobe啟用工程師。 如果您未直接與該人員合作，則可以從您的Adobe代表瞭解要將檔案傳送給誰。
   * Adobe同時支援主要和備份憑證。 如果您的主要憑證遭到任何危害，您可以撤銷該憑證，然後切換至次要憑證，在您的應用程式中簽署請求者ID。 這將確保在生產中憑證之間的順利轉換，並將對客戶的影響降至最低。
   * 一旦Adobe收到PEM檔案，驗證工程師會將其新增至伺服器端的設定，並確認檔案已收到。
