# Week 3 Study Guide: Tokens and Two-Factor Authentication

## Course Context: Security & Data Systems

**Week 3 Focus:** Tokens and Two-Factor Authentication  
**Previous:** Authentication Fundamentals → Biometric  
**Next:** FIDO Alliance and Standards → Access Control

---

## Learning Objectives

By the end of Week 3, you should master:
- Two-factor authentication principles and implementation patterns
- Hardware token technologies and their security characteristics
- Mobile-based authentication mechanisms (SMS, apps, QR codes)
- Google's device-centric authentication approach and lessons learned
- Channel binding and cryptographic enhancement techniques
- Privacy considerations in multi-factor authentication systems

---

## Two-Factor Authentication Fundamentals

### Definition & Security Model
**Two-Factor Authentication (2FA)** = Authentication using two different factor types from:
- Something you **know** (password, PIN)
- Something you **have** (token, phone, smart card)
- Something you **are** (biometric characteristics)
- Something about **context** (location, behavior patterns)

### Security Foundation
```
Single Factor: Password only
    ↓
Vulnerable to: theft, guessing, phishing, reuse

Multi-Factor: Password + Token
    ↓
Requires: compromise of multiple independent factors
```

### Attack Resistance Properties
- **Phishing resistance**: Tokens bound to specific services
- **Credential stuffing prevention**: Unique tokens per service
- **Replay attack mitigation**: Time-limited or challenge-based tokens
- **Man-in-the-middle protection**: Cryptographic binding to channels

---

## Hardware Authentication Tokens

### YubiKey Technology Deep Dive

#### **Core Architecture:**
- **Hardware Security Module**: Tamper-resistant cryptographic processor
- **USB HID interface**: Keyboard emulation, no drivers required
- **Capacitive touch sensor**: Physical presence verification
- **Secure element**: Hardware-protected key storage

#### **Operational Modes:**

##### **YubiKey OTP Mode:**
```
Button Press → Generate 44-character OTP
Format: [device_id][session_counter][timestamp][random][CRC]
Server validates using shared AES key
```

##### **OATH-HOTP Mode (RFC 4226):**
```
Counter-based: HOTP = HMAC-SHA1(K, C)
K = shared secret key
C = counter value (incremented each use)
```

##### **Challenge-Response Mode:**
```
1. Server sends 64-byte challenge
2. YubiKey computes HMAC-SHA1(secret, challenge)
3. Returns 20-byte response
4. Server verifies using shared secret
```

##### **Static Password Mode:**
- Stores 38-character static password
- Keyboard output when button pressed
- Legacy system compatibility

#### **Advanced Features:**
- **Two independent slots**: Different configurations per slot
- **NFC capability**: Contactless operation (YubiKey NEO)
- **PIV/Smart Card**: X.509 certificate storage
- **FIDO U2F/U2A**: WebAuthn compatibility

#### **Enterprise Deployment:**
- **Centralized management**: YubiKey management software
- **HSM integration**: Hardware Security Module for server secrets
- **Attestation certificates**: Verify authentic YubiKey hardware
- **Bulk provisioning**: Large-scale enterprise deployment

### Alternative Hardware Tokens

#### **Multi-Application USB Tokens:**
- **FIDO U2F compliance**: Universal 2nd Factor standard
- **NFC communication**: Near Field Communication support
- **Java Card platform**: Multiple applications per device
- **Cross-platform drivers**: Windows, Linux, macOS, Android

#### **Form Factor Variations:**
- **USB-A/USB-C**: Standard computer connectivity
- **Lightning connector**: iOS device compatibility
- **NFC rings/cards**: Wearable form factors
- **Bluetooth tokens**: Wireless operation

---

## Mobile Phone as Authentication Token

### SMS-Based Authentication

#### **Implementation Architecture:**
```
1. User initiates login with username/password
2. Server generates random 6-8 digit code
3. Server sends SMS to registered phone number
4. User enters code within time window (60-300 seconds)
5. Server validates code and grants access
```

#### **Security Analysis:**

##### **Strengths:**
- **Universal availability**: Every phone supports SMS
- **No app installation**: Works on basic phones
- **Familiar user experience**: Widely understood process
- **Infrastructure leverage**: Uses existing telecom networks

##### **Vulnerabilities:**

###### **SS7 Protocol Weaknesses:**
- **Network dating to 1970s**: Legacy signaling system
- **No encryption**: Messages transmitted in clear text
- **89% SMS interception rate** in security research
- **58% location tracking capability**
- **50% call wiretapping possibility**

###### **SIM Swapping Attacks:**
- **Social engineering**: Attacker convinces carrier to transfer number
- **Identity theft**: Using personal information to authenticate
- **Port-out scams**: Unauthorized number transfers
- **Insider threats**: Malicious telecom employees

###### **Stingray/IMSI Catcher Attacks:**
- **Fake cell towers**: Force phones to connect to attacker equipment
- **Man-in-the-middle**: Intercept SMS messages
- **Location tracking**: Monitor user movements
- **Selective targeting**: Focus on specific phone numbers

#### **Cost and Usability Issues:**
- **International roaming**: Expensive SMS charges abroad
- **Delivery delays**: Network congestion causing timeouts
- **No cellular coverage**: Rural or indoor dead zones
- **Multiple devices**: Difficulty with tablets, laptops

### Smartphone App-Based Authentication

#### **Google Authenticator Implementation**

##### **Technical Foundation:**
```
TOTP Algorithm (RFC 6238):
TOTP = HOTP(K, T)
where:
K = shared secret key (160-bit)
T = floor((Current Time - T0) / Time Step)
T0 = Unix epoch (January 1, 1970)
Time Step = 30 seconds
```

##### **Setup Process:**
```
1. Server generates random 160-bit secret
2. Secret encoded in QR code with service info
3. User scans QR code with authenticator app
4. App stores secret locally on device
5. Server stores secret associated with user account
```

##### **Authentication Flow:**
```
1. User requests access
2. Server requests TOTP code
3. App generates 6-digit code using current time
4. User enters code
5. Server computes expected code and validates
6. Access granted if codes match (within time window)
```

#### **QR Code Technology**

##### **ISO/IEC 18004:2006 Standard:**
- **Error correction**: Reed-Solomon error correction
- **Data capacity**: 
  - Numeric: 7,089 characters maximum
  - Alphanumeric: 4,296 characters maximum
  - Binary: 2,953 bytes maximum

##### **Secret Sharing Format:**
```
otpauth://totp/Service:user@example.com?secret=BASE32SECRET&issuer=Service&algorithm=SHA1&digits=6&period=30
```

#### **Security Advantages:**
- **Offline operation**: No network connectivity required
- **Cryptographic security**: HMAC-SHA1 based
- **Time synchronization**: Automatic with device clock
- **No SMS vulnerabilities**: Eliminates telecom attack vectors

#### **Deployment Considerations:**
- **Clock synchronization**: Device time accuracy critical
- **Secret backup**: Recovery if device lost or reset
- **Multiple devices**: Sharing secrets across devices
- **Enterprise management**: Bulk secret provisioning

---

## QR-Login and Advanced Mobile Authentication

### QR-Login Architecture

#### **Protocol Flow:**
```
1. Desktop browser displays QR code with session ID
2. Mobile app scans QR code
3. App extracts session information
4. App authenticates to server using stored credentials
5. Server links mobile session to desktop session
6. Desktop automatically logged in
```

#### **WhatsApp Web Example:**
```
1. User visits web.whatsapp.com
2. Page displays QR code containing:
   - Session identifier
   - Server public key
   - Challenge nonce
3. WhatsApp mobile app scans code
4. App validates server identity
5. App signs challenge with device private key
6. Web session established
```

### Security Benefits of QR-Login

#### **Attack Resistance:**
- **Keylogger immunity**: No password typing required
- **Shoulder surfing protection**: QR code scanning discrete
- **Phishing resistance**: Mobile app validates server identity
- **Man-in-the-middle prevention**: Cryptographic server binding

#### **Usability Advantages:**
- **Speed**: Faster than typing credentials
- **Complex passwords**: No memorization requirement
- **Untrusted computers**: Safe to use public terminals
- **Cross-site privacy**: Different credentials per service

#### **Privacy Enhancement:**
- **Unlinkable accounts**: Random credentials per service
- **Username randomization**: No personal information required
- **Session isolation**: Desktop and mobile sessions independent

---

## Google's Device-Centric Authentication Model

### Evolution from User-Centric to Device-Centric

#### **Traditional Model Problems:**
```
User Authentication:
User → Password → Server
Issues: Password reuse, weak passwords, phishing
```

#### **Device-Centric Solution:**
```
Device Authorization:
Device → Certificate → Server
Benefits: Strong crypto, device tracking, easy revocation
```

### Two-Step Verification (2SV) Implementation

#### **Initial Deployment (2010-2012):**
- **Opt-in system**: User chooses to enable 2SV
- **Monthly verification**: 30-day trusted computer cookies
- **Multiple delivery methods**: SMS, voice, app, backup codes
- **Scale**: Millions of users, largest 2FA deployment

#### **Code Delivery Mechanisms:**

##### **SMS/Voice Delivery:**
- **Primary method**: 90% of users choose SMS
- **International support**: Global telecom integration
- **Accessibility**: Voice calls for hearing impaired
- **Backup numbers**: Multiple phone number registration

##### **Mobile Application:**
- **Google Authenticator**: TOTP-based code generation
- **Offline capability**: No cellular service required
- **Multiple accounts**: Single app for multiple services
- **QR code setup**: Easy initial configuration

##### **Backup Codes:**
- **Pre-generated**: 10 single-use codes
- **Printable**: Physical backup option
- **Recovery mechanism**: When primary methods unavailable
- **Regeneration**: New codes replace used ones

#### **User Experience Lessons:**

##### **Adoption Patterns:**
- **Security incident driven**: Mat Honan case study
- **250,000 new users in 2 days** after public breach
- **Government officials**: High-value target adoption
- **Enterprise**: Mandatory 2FA for sensitive roles

##### **Support Challenges:**
- **International travel**: SMS delivery issues
- **Device replacement**: Phone number portability
- **Account recovery**: Customer support requirements
- **Application compatibility**: Legacy IMAP/POP clients

### Mental Model Evolution

#### **From User Authentication to Device Authorization:**

##### **Original 2SV Design (2010):**
```
Purpose: User authentication enhancement
Method: Periodic verification (30 days)
Problem: Too frequent OR too long vulnerability window
```

##### **Revised Model (2012+):**
```
Purpose: Device authorization and blessing
Method: Permanent trusted device status
Benefit: Device-based access control and monitoring
```

#### **Device Blessing Process:**
```
1. New device login attempt
2. Primary authentication (username/password)
3. Second factor challenge (SMS/app/token)
4. User confirms "trust this device"
5. Device receives permanent authorization cookie
6. Future logins skip second factor
7. User can revoke device authorization anytime
```

### Application-Specific Passwords (ASP)

#### **Legacy Application Problem:**
- **IMAP/POP clients**: No 2SV support
- **Mobile apps**: Basic auth only
- **API access**: Programmatic authentication

#### **ASP Solution:**
```
1. User generates ASP in account settings
2. System creates random 16-character password
3. Application uses ASP instead of account password
4. ASP provides full account access (not application-limited)
5. User can revoke individual ASPs
```

#### **ASP Limitations:**
- **Full account access**: Not application-scoped
- **User confusion**: Misunderstanding of permissions
- **Phishing vulnerable**: Can be stolen like passwords
- **Management overhead**: Multiple passwords to track

### Android Integration Model

#### **Centralized Account Management:**
```
1. User signs in through browser flow
2. System handles 2SV challenge
3. Account Manager stores tokens
4. Apps request scoped tokens from Account Manager
5. No password sharing between applications
```

#### **Benefits:**
- **Single sign-on**: One authentication for multiple apps
- **Token scoping**: Limited permissions per application
- **Centralized revocation**: Disable all app access at once
- **No ASP required**: Native 2SV support

---

## Advanced Cryptographic Authentication

### Smartcard-Like USB Tokens

#### **Design Requirements:**
- **No software installation**: Browser-only operation
- **Cross-platform compatibility**: Windows, macOS, Linux
- **Privacy preservation**: No cross-site tracking
- **Open standards**: Vendor-independent protocols

#### **Technical Implementation:**

##### **Public Key Cryptography:**
```
Registration:
1. Browser requests key generation
2. Token generates keypair in secure element
3. Public key sent to server
4. Private key never leaves token
5. Server stores public key with user account

Authentication:
1. Server sends cryptographic challenge
2. Browser forwards to token
3. Token signs challenge with private key
4. Signature returned to server
5. Server verifies using stored public key
```

##### **Hardware Security Features:**
- **Secure element**: Tamper-resistant key storage
- **Attestation certificates**: Verify genuine hardware
- **User presence**: Capacitive touch confirmation
- **Anti-tampering**: Hardware destruction if compromised

#### **Privacy Protection Mechanisms:**

##### **Key Generation Per Site:**
```
For each registration:
1. Generate new keypair
2. Never reuse existing keys
3. Bind keys to specific website
4. Prevent cross-site correlation
```

##### **Website Binding:**
```
1. Browser provides hashed website identifier to token
2. Token includes website hash in signature
3. Prevents credential sharing between sites
4. Resists datacenter intrusion attacks
```

### Channel ID and Cookie Binding

#### **Problem with Cookie-Based Authentication:**
- **Bearer tokens**: Cookies stolen = account compromised
- **JavaScript access**: XSS can steal authentication cookies
- **Network sniffing**: Unencrypted cookie transmission

#### **Channel ID Solution:**

##### **Automatic Key Generation:**
```
1. Browser visits new domain
2. Automatically generates P-256 EC keypair
3. Uses private key for all SSL connections to domain
4. Public key identifies client to server
5. Cookies bound to client public key
```

##### **Cryptographic Binding:**
```
Cookie Creation:
1. Server receives client public key during SSL handshake
2. Server binds authentication cookie to public key hash
3. Cookie only valid when presented with matching private key

Cookie Usage:
1. Browser presents cookie in HTTP request
2. SSL layer proves possession of private key
3. Server validates cookie binding to public key
4. Authentication succeeds only with both components
```

#### **Security Benefits:**
- **Malware resistance**: Private key in OS keystore
- **Replay prevention**: SSL session binding
- **Phishing protection**: Domain-specific key generation
- **Zero user interface**: Transparent security enhancement

#### **TPM Integration:**
- **Hardware key storage**: Trusted Platform Module protection
- **Performance optimization**: Dedicated cryptographic processor
- **Enterprise deployment**: IT management capabilities
- **Laptop security**: Hardware-bound device identity

---

## Server-Side Authentication Infrastructure

### Risk-Based Authentication

#### **Behavioral Analysis:**
```
Risk Factors:
- Geographic location patterns
- Time-of-day analysis
- Device fingerprinting
- Network characteristics
- User behavior patterns
```

#### **Adaptive Response:**
```
Low Risk: Single factor authentication
Medium Risk: Step-up to second factor
High Risk: Additional verification + alert
Critical Risk: Account lockdown + investigation
```

### Certificate Transparency

#### **Problem:**
- **CA compromise**: Diginotar, Comodo incidents
- **Rogue certificates**: Undetected fraudulent certificates
- **Government coercion**: Forced certificate issuance

#### **Solution:**
```
1. All certificates published in public logs
2. Cryptographic proof of publication required
3. Domain owners monitor logs for unauthorized certificates
4. Independent auditors verify log integrity
5. Browser policies enforce certificate transparency
```

### Service Account Authentication

#### **Cloud Infrastructure Identity:**
```
1. Application requests authentication token
2. Cloud platform signs assertion with private key
3. Target service validates using platform public key
4. Automatic credential rotation
5. No password storage required
```

#### **OAuth 2.0 Integration:**
```
1. Service account requests OAuth token
2. Cloud platform provides signed JWT
3. Token scoped to specific APIs/resources
4. Time-limited validity (1 hour default)
5. Automatic refresh before expiration
```

---

## Privacy Considerations in Multi-Factor Authentication

### User Privacy Principles

#### **Data Minimization:**
- **Local verification**: Biometrics never leave device
- **Minimal collection**: Only necessary identifiers
- **Purpose limitation**: Authentication-only use
- **Retention limits**: Automatic data expiration

#### **Unlinkability:**
- **Per-service credentials**: Unique keys per relying party
- **No global identifiers**: Cannot correlate across services
- **Batch attestation**: 100,000+ devices share certificates
- **Anonymous analytics**: Aggregate statistics only

### Regulatory Compliance

#### **GDPR Requirements:**
- **Lawful basis**: Legitimate interests for security
- **Data subject rights**: Access, portability, erasure
- **Privacy by design**: Built-in protection mechanisms
- **Impact assessments**: High-risk processing evaluation

#### **Sector-Specific Regulations:**
- **PCI DSS**: Payment card authentication requirements
- **HIPAA**: Healthcare data protection
- **SOX**: Financial audit trail requirements
- **FERPA**: Educational record protection

---

## Real-World Deployment Patterns

### Enterprise Implementation

#### **Phased Rollout Strategy:**
```
Phase 1: IT administrators and security team
Phase 2: Executives and privileged users  
Phase 3: All employees with external access
Phase 4: Internal systems and applications
Phase 5: Full organization coverage
```

#### **User Training and Support:**
- **Security awareness**: Threat landscape education
- **Technology training**: How to use 2FA tools
- **Help desk preparation**: Support for common issues
- **Emergency procedures**: Account recovery processes

### Consumer Service Deployment

#### **Opt-in vs Mandatory:**
- **High-value accounts**: Mandatory for financial services
- **General services**: Opt-in with incentives
- **Risk-based triggers**: Automatic enrollment after incidents
- **Granular controls**: Per-feature 2FA requirements

#### **User Experience Optimization:**
- **Progressive disclosure**: Gradual complexity introduction
- **Multiple options**: SMS, app, hardware token choices
- **Recovery mechanisms**: Multiple backup methods
- **Clear communication**: Security benefit explanation

---

## Key Concepts Summary

### Critical Technologies
- **TOTP/HOTP**: Time and counter-based one-time passwords
- **QR codes**: Visual encoding for secret sharing
- **Public key cryptography**: Asymmetric authentication
- **Channel binding**: Cryptographic session protection
- **Hardware security modules**: Tamper-resistant key storage

### Design Principles
1. **Usability first**: Security that users will actually use
2. **Defense in depth**: Multiple independent factors
3. **Privacy by design**: Built-in protection mechanisms
4. **Open standards**: Interoperability and competition
5. **Risk-based adaptation**: Context-appropriate security

### Deployment Considerations
- **User education**: Training and support requirements
- **Technical integration**: Legacy system compatibility
- **Cost analysis**: Hardware, support, and maintenance costs
- **Regulatory compliance**: Legal and industry requirements
- **Incident response**: Compromise detection and recovery

---

## Practice Questions

### Conceptual Understanding:
1. Explain why SMS-based 2FA is considered less secure than app-based authentication.
2. How does device-centric authentication differ from traditional user-centric models?
3. What privacy protection mechanisms prevent cross-site tracking in FIDO tokens?

### Applied Analysis:
1. Design a 2FA implementation for a healthcare organization. Consider HIPAA compliance and user workflow.
2. Compare the security and usability trade-offs between hardware tokens and smartphone apps.
3. How would you implement secure account recovery for users who lose their 2FA device?

### Critical Thinking:
1. How might quantum computing impact current cryptographic authentication methods?
2. What authentication challenges emerge in IoT devices with limited user interfaces?
3. How do you balance security requirements with accessibility needs for disabled users?

### Technical Implementation:
1. Design the protocol flow for QR-code based login system.
2. Explain how Channel ID prevents cookie theft while maintaining user privacy.
3. What considerations are needed for deploying 2FA across multiple geographic regions?