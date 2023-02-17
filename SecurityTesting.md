# Security Testing Tools


| Tools              | Type | Responsibility                                                              |
| ------------------ | ---- | --------------------------------------------------------------------------- |
| App-scan           | DAST | Cross-Site Scripting (XSS), SQL injection                                   |
| ZAP                | DAST | Cross-Site Scripting (XSS), SQL injection                                   |
| dependency checker | SAST | dependency scan and its vulnerability check against global vulnerability DB |
| nessus scan        | DAST | misconfigured settings, outdated software, and unpatched systems            |


# TLS

![image](https://user-images.githubusercontent.com/55741060/219594021-d4d55bca-f9a6-4dc4-9252-98330b6dabe5.png)

# OWASP

![image](https://user-images.githubusercontent.com/55741060/219593896-7a72e12f-a437-457f-8f88-b6aae2334d30.png)

# SSL	vs TLS

| SSL                                              | TLS                                |
| ------------------------------------------------ | ---------------------------------- |
| deprecated                                       | latest                             |
| RC4 encrption                                    | SHA-256 and other latest encrytion |
| handshake and communication pattern is different |

# Security Issues

| Issue             | Description                | mitigation         |
| ----------------- | -------------------------- | ------------------ |
| CSRF              | cross site request forgery | csrfgaurd.jar,CORS |
| XSS               | cross site scripting       | antisamy.jar,CORS  |
| SQL Injection     |                            |                    |
| middle man attack |                            | TLS,HTTPS          |



