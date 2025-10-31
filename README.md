# Intune Android MDM Deployment (Company-Owned)

## Objective

This project demonstrates the deployment and configuration of Microsoft Intune for corporate-owned Android devices.
The goal was to create a secure, standardized, and user-friendly mobile device management environment for company-owned endpoints which includes device configuration, compliance enforcement, and application management.
The deployment was tested and validated on Samsung Galaxy A55, A54, and A14 devices.

### Skills Learned

- Implementation of Android Enterprise Fully Managed deployment model.
- Configuration of Intune device and compliance policies for Android.
- Deployment and customization of Microsoft Launcher for corporate use.
- App lifecycle management using Managed Google Play and App Configuration Policies.
- Hands-on experience with QR token–based enrollment for Android devices.
- Practical knowledge of device hardening and CIS Benchmark alignment for mobile endpoints.

### Tools Used

- Microsoft Intune / Endpoint Manager Admin Center
- Android Enterprise with Managed Google Play
- Microsoft Launcher (corporate customization)
- Company Portal / Intune App
- CIS Google Android Benchmark v1.5.0 manual/documentation?

## Steps

#### 1. Connect Managed Google Play

Before configuring any Android policies, the Intune tenant must be linked with Managed Google Play.

<img width="2319" height="507" alt="image" src="https://github.com/user-attachments/assets/7c2a63e0-ba02-4da2-bacb-d5116a5eb3fc" />

*Ref 1: Managed Google Play Account*

#### 2. Create Enrollment Profile

Create a new enrollment profile and configure token validity for Corporate-owned, fully managed user devices

<img width="812" height="400" alt="image" src="https://github.com/user-attachments/assets/7632ed83-c859-4182-80ec-ae0e19e3f78c" />

*Ref 2: Enrollment Token*

#### 3. Configuration Policy Creation

A dedicated Device Restrictions profile was created in Microsoft Intune under the Android Enterprise (Fully Managed) platform, following the CIS Google Android Benchmark v1.5.0 Level 1 recommendations.
This configuration ensures a hardened and standardized security posture across all corporate-owned devices.

| Category              | Setting                   | Value                                         |
| --------------------- | ------------------------- | --------------------------------------------- |
| **General**           | Date & Time changes       | Block                                         |
|                       | Wi-Fi configuration       | Block                                         |
|                       | Bluetooth configuration   | Block                                         |
|                       | USB file transfer         | Block                                         |
|                       | External media            | Block                                         |
|                       | NFC / Beam data           | Block                                         |
|                       | Factory reset             | Block                                         |
|                       | Locate device             | Allow                                         |
|                       | Power button menu         | Block                                         |
|                       | System updates            | Maintenance window (22:30 – 12:00)            |
| **Security**          | Threat scan on apps       | Require                                       |
|                       | Common Criteria mode      | Require                                       |
| **Device Experience** | Launcher                  | Microsoft Launcher (fully managed)            |
|                       | Make default launcher     | Enable                                        |
| **Password**          | Required type             | Weak biometric (PIN/pattern)                  |
|                       | Expiration                | 30 days                                       |
|                       | Password history          | 3 previous                                    |
|                       | Auto lock                 | 1 minute                                      |
|                       | Unlock frequency          | 24 hours                                      |
|                       | Lock-screen notifications | Hidden                                        |
| **Users & Accounts**  | Add new users             | Block                                         |
|                       | User removal              | Block                                         |
|                       | Personal Google accounts  | Block                                         |
| **Support Info**      | Lock screen message       | “If found, contact IT Service Desk.”          |
|                       | Short support message     | “You do not have permission for this action.” |

<img width="1258" height="706" alt="image" src="https://github.com/user-attachments/assets/33bd2744-3100-4f59-ae67-eb608e609dbf" />

*Ref 3: Policy configuration*

#### 4. Compliance Policy Setup

Created Compliance Policies to enforce security baselines:
- Minimum Android OS version
- Require PIN/password with complexity rules
- Require encryption
- Block rooted/jailbroken devices
- Enforce secure lock screen timeout
Non-compliant devices were automatically flagged and blocked from accessing M365 resources through Conditional Access.

| Category                            | Setting                                 | Value                          |
| ----------------------------------- | --------------------------------------- | ------------------------------ |
| **Device Health**                   | Rooted devices                          | Block                          |
|                                     | Device threat level                     | High                           |
|                                     | Play Integrity verdict                  | Check basic & device integrity |
| **Device Properties**               | Minimum OS version                      | 15                             |
| **System Security**                 | Require password to unlock              | Required                       |
|                                     | Required password type                  | At least numeric               |
|                                     | Minimum password length                 | 6                              |
|                                     | Password expiry (days)                  | 30                             |
|                                     | Password history                        | 3                              |
|                                     | Max inactivity before lock              | 1 minute                       |
|                                     | Encryption of data storage              | Required                       |
|                                     | Intune app runtime integrity            | Required                       |
| **Microsoft Defender for Endpoint** | Require device under machine risk score | High                           |
| **Noncompliance Actions**           | Mark device noncompliant                | Immediately                    |
|                                     | Add device to retire list               | After 3 days                   |

<img width="627" height="1000" alt="image" src="https://github.com/user-attachments/assets/738c4b9d-fb57-45fa-b515-73dc62a6b750" />

*Ref 4: Compliance Policy*

#### 5. Application Configuration & Deployment

Added core corporate apps from Managed Google Play:
All apps were assigned as Required ensuring automatic installation after enrollment.
Using Intune App Configuration Policies, selected apps (such as Outlook, Teams, and Authenticator) can be preconfigured for example for SSO login using Entra ID credentials or automatic email account setup.
This guarantees that after enrollment, every device has all essential M365 apps pre-installed, properly configured, and secured.

<img width="970" height="525" alt="image" src="https://github.com/user-attachments/assets/42d925f0-d905-44c3-b101-742bda18178e" />

*Ref 5: Aplication deployment*
