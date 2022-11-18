# Azure Authentication - OAuth 2.0

---

- Tokens are at the center of OAuth 2.0 identity platforms, such as Azure Active Directory (Azure AD). 
- To access a resource (for example, a web application protected by Azure AD), a user must present a valid token. 
- To obtain that token, the user must sign into Azure AD using their credentials. 
- At that point, depending on policy, they may be required to complete MFA. The user then presents that token to the web application, which validates the token and allows the user access.

![image](https://user-images.githubusercontent.com/90521014/202799931-ba06858e-993d-4003-9c58-18a796cc7b0d.png)

- When Azure AD issues a token, **it contains information (claims) such as 
  - **the username, source IP address, MFA**, ... 
  - **any privilege a user has in Azure AD**. 

If you sign in as a Global Administrator to your Azure AD tenant, then the token will reflect that. 

Two of the most common token theft techniques DART has observed have been through:

- adversary-in-the-middle (AitM) frameworks
- the utilization of commodity malware (which enables a **pass-the-cookie** scenario)

---

## References

- [MSDART - Token tactics: How to prevent, detect, and respond to cloud token theft](https://www.microsoft.com/en-us/security/blog/2022/11/16/token-tactics-how-to-prevent-detect-and-respond-to-cloud-token-theft/)
