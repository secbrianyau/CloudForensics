# Azure Authentication - OAuth 2.0

Contents

- [Azure Authentication - OAuth 2.0](#azure-authentication---oauth-20)
  - [OAuth Token](#oauth-token)
  - [2FA Defense to traditional phishing](#2fa-defense-to-traditional-phishing)
  - [Adversary-in-the-middle (AitM) phishing attack](#adversary-in-the-middle-aitm-phishing-attack)
  - [Pass-the-cookie attack](#pass-the-cookie-attack)
  - [References](#references)

---

## OAuth Token

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

## 2FA Defense to traditional phishing

![image](https://user-images.githubusercontent.com/90521014/202800632-b2f05964-0b0f-4129-adcb-b918328e5875.png)

With traditional credential phishing, the attacker may use the credentials they have compromised to try and sign in to Azure AD. If the security policy requires MFA, the attacker is halted from being able to successfully sign in. Though the users' credentials were compromised in this attack, the threat actor is prevented from accessing organizational resources.

---

## Adversary-in-the-middle (AitM) phishing attack

- Attacker methodologies are always evolving, and to that end DART has seen an increase in attackers using AitM techniques to steal tokens instead of passwords. 
- Frameworks like **Evilginx2** go far beyond credential phishing, by inserting malicious infrastructure between the user and the legitimate application the user is trying to access. 
- When the user is phished, **the malicious infrastructure captures both the credentials of the user, and the token**.

![image](https://user-images.githubusercontent.com/90521014/202800788-0c54cfcb-eb2a-4c5b-b23d-fa823ea9a7fd.png)

- If a regular user is phished and their token stolen, the attacker may attempt business email compromise (BEC) for financial gain. 
- If a token with Global Administrator privilege is stolen, then they may attempt to take over the Azure AD tenant entirely, resulting in loss of administrative control and total tenant compromise.

---

## Pass-the-cookie attack

- A **pass-the-cookie** attack is a type of attack where an attacker can bypass authentication controls by compromising browser cookies.
- At a high level, browser cookies allow web applications to store user authentication information. This allows a website to keep you signed in and not constantly prompt for credentials every time you click a new page.
- **Pass-the-cookie** is like **pass-the-hash** or **pass-the-ticket** attacks in Active Directory. After authentication to Azure AD via a browser, a cookie is created and stored for that session. 
- If an attacker can **compromise a device and extract the browser cookies**, they could pass that cookie into a separate web browser on another system, **bypassing security checkpoints along the way**. 
- Users who are accessing corporate resources **on personal devices** are especially at risk. Personal devices often have weaker security controls than corporate-managed devices and IT staff lack visibility to those devices to determine compromise. 
  - They also have additional attack vectors, such as personal email addresses or social media accounts users may access on the same device. Attackers can compromise these systems and steal the authentication cookies associated with both personal accounts and the users' corporate credentials.

![image](https://user-images.githubusercontent.com/90521014/202801094-601a83d2-d7c9-4c19-b9d8-1801c2d622ae.png)

- Commodity credential theft malware like Emotet, Redline, IcedID, and more all have built-in functionality to extract and exfiltrate browser cookies. 
- Additionally, the attacker does not have to know the compromised account password or even the email address for this to work — those details are held within the cookie.

---

## Protection

- Full visibility of where and how their users are authenticating
- To access critical applications like Exchange Online or SharePoint, the device used should be known by the organization
  - Device control (e.g. InTune): make sure devices are well-protected by patch manager, EDR, AV, etc
  - Device-based access conditional control policies
  - [Allow known-devices only](https://learn.microsoft.com/en-us/azure/active-directory/conditional-access/howto-conditional-access-policy-compliant-device)
  - [Intune Security Baseline](https://learn.microsoft.com/en-us/mem/intune/protect/security-baselines)
- Session conditional access policies and other compensating controls to reduce the impact of token theft
  - [Reducing the lifetime of the session](https://learn.microsoft.com/en-us/azure/active-directory/conditional-access/concept-conditional-access-session)
    - Reducing the viable time of a token  
  - [Conditional Access App Control in Microsoft Defender for Cloud Apps](https://learn.microsoft.com/en-us/defender-cloud-apps/proxy-intro-aad)
- Risk-based Approach:
  - Highly privileged users like Global Administrators, Service Administrators, Authentication Administrators, and Billing Administrators among others.
  - Finance and treasury type applications that are attractive targets for attackers seeking financial gain.
  - Human capital management (HCM) applications containing personally identifiable information that may be targeted for exfiltration.
  - Control and management plane access to Microsoft 365 Defender, Azure, Office 365 and other cloud app administrative portals.
  - Access to Office 365 services (Exchange, SharePoint, and Teams) and productivity-based cloud apps.
  - VPN or remote access portals that provide external access to organizational resources. 

---

## References

- [MSDART - Token tactics: How to prevent, detect, and respond to cloud token theft](https://www.microsoft.com/en-us/security/blog/2022/11/16/token-tactics-how-to-prevent-detect-and-respond-to-cloud-token-theft/)
