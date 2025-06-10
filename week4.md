# Week 4 Study Guide: FIDO Alliance and Standards

## Course Context: Security & Data Systems

**Week 4 Focus:** FIDO Alliance and Standards  
**Previous:** Authentication Fundamentals → Biometric → Tokens and Two-Factor  
**Next:** Access Control

---

## Learning Objectives

By the end of Week 4, you should master:
- FIDO Alliance mission, goals, and ecosystem evolution
- FIDO UAF (Universal Authentication Framework) architecture and protocols
- FIDO U2F (Universal Second Factor) implementation and security
- FIDO2 and WebAuthn standards for modern web authentication
- Federation integration patterns and enterprise deployment strategies
- Privacy principles and security measures in FIDO implementations

---

## FIDO Alliance Overview

### Mission and Formation
**FIDO (Fast IDentity Online) Alliance** = Industry consortium formed July 2012 to revolutionize online authentication

### Core Problems Addressed
- **Password security weaknesses**: Reuse, theft, phishing vulnerability
- **Authentication fragmentation**: Proprietary solutions, vendor lock-in
- **Poor user experience**: Complex passwords, multiple credentials
- **Scalability challenges**: Enterprise deployment complexity

### Design Principles
1. **Ease of use**: Simple, intuitive authentication experience
2. **Privacy and security**: Strong cryptographic protection without PII exposure
3. **Standardization**: Open, interoperable protocols across vendors
4. **Device agnostic**: Works across platforms and form factors

### Industry Impact
- **250+ member companies** spanning the authentication ecosystem
- **Standardized protocols** adopted by major browsers and platforms
- **Real-world deployments** by Google, PayPal, Samsung, Microsoft
- **Government adoption** including US NSTIC/NIST and UK Cabinet Office

---

## FIDO UAF (Universal Authentication Framework)

### Overview and Goals
**UAF** = Passwordless authentication protocol enabling strong, user-friendly authentication

### High-Level Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   User Device   │    │  Relying Party   │    │ FIDO Alliance   │
│                 │    │                  │    │                 │
│ ┌─────────────┐ │    │ ┌──────────────┐ │    │ ┌─────────────┐ │
│ │FIDO UAF     │◄┼────┼►│FIDO UAF      │ │    │ │ Metadata    │ │
│ │Client       │ │    │ │Server        │ │    │ │ Service     │ │
│ └─────────────┘ │    │ └──────────────┘ │    │ └─────────────┘ │
│        │        │    │        │         │    └─────────────────┘
│ ┌─────────────┐ │    │ ┌──────────────┐ │
│ │Authenticator│ │    │ │Relying Party │ │
│ │             │ │    │ │Web Server    │ │
│ └─────────────┘ │    │ └──────────────┘ │
└─────────────────┘    └──────────────────┘
```

### Core Components

#### **FIDO UAF Client**
- **Responsibility**: Client-side protocol implementation
- **Functions**:
  - Communicates with authenticators via ASM (Authenticator Specific Module)
  - Interfaces with user agents (browsers, mobile apps)
  - Manages FIDO UAF message flow

#### **FIDO UAF Server** 
- **Responsibility**: Server-side protocol implementation
- **Functions**:
  - Validates authenticator attestations using metadata
  - Manages user account associations with registered authenticators
  - Evaluates authentication and transaction confirmation responses
  - Can be deployed on-premises or as cloud service

#### **FIDO UAF Authenticator**
- **Responsibility**: Secure key generation and authentication operations
- **Capabilities**:
  - Creates unique key pairs per relying party
  - Performs user verification (biometric, PIN, presence)
  - Provides attestation of authenticator type and capabilities
  - Supports transaction confirmation with secure display

#### **Authenticator Abstraction Layer**
- **Purpose**: Uniform API for diverse authenticator types
- **Benefits**:
  - Enables multi-vendor authenticator support
  - Abstracts implementation details from clients
  - Facilitates plugin architecture for new authenticator types

### FIDO UAF Protocol Operations

#### **1. Registration Process**
```
1. User initiates registration with relying party
2. Server sends registration policy and challenge
3. Client discovers available authenticators
4. User selects and enrolls with chosen authenticator
5. Authenticator generates unique key pair for RP
6. Public key and attestation sent to server
7. Server validates attestation and stores public key
```

#### **2. Authentication Process**
```
1. User initiates authentication
2. Server sends authentication challenge and policy
3. Client presents authentication options to user
4. User performs required verification (biometric/PIN)
5. Authenticator signs challenge with private key
6. Signed response sent to server
7. Server validates signature using stored public key
```

#### **3. Transaction Confirmation**
```
1. Relying party requests transaction confirmation
2. Transaction details displayed on secure authenticator display
3. User reviews and approves transaction (WYSIWYS - What You See Is What You Sign)
4. Authenticator signs transaction details
5. Server validates transaction signature
```

#### **4. Deregistration**
```
1. Relying party initiates deregistration
2. Server requests authenticator to delete credentials
3. Authenticator removes associated private key
4. Server removes public key from user account
```

### Authenticator Types and Capabilities

#### **First-Factor Authenticators**
- **Function**: Complete authentication without additional factors
- **Examples**: Biometric authenticators, PIN-based authenticators
- **Use cases**: Primary authentication for mobile apps, desktop login

#### **Second-Factor Authenticators**
- **Function**: Supplement password-based authentication
- **Examples**: USB tokens, NFC devices, mobile apps
- **Use cases**: Enterprise 2FA, high-value transaction protection

#### **Roaming vs Bound Authenticators**
- **Roaming**: Portable across devices and platforms
- **Bound**: Tied to specific device or platform
- **Trade-offs**: Convenience vs security, cost vs deployment complexity

---

## FIDO U2F (Universal Second Factor)

### Overview and Purpose
**U2F** = Simple, standardized second-factor authentication protocol

### Design Goals
- **Phishing protection**: Cryptographic origin binding
- **Privacy preservation**: No trackable identifiers across services
- **Universal compatibility**: One token for multiple services
- **Ease of deployment**: Minimal integration complexity

### U2F Architecture

```
┌─────────────────┐    ┌──────────────────┐
│   User Device   │    │  Relying Party   │
│                 │    │                  │
│ ┌─────────────┐ │    │ ┌──────────────┐ │
│ │ Web Browser │◄┼────┼►│    Web App   │ │
│ │  (Chrome)   │ │    │ │              │ │
│ └─────────────┘ │    │ └──────────────┘ │
│        │        │    │        │         │
│        ▼        │    │        ▼         │
│ ┌─────────────┐ │    │ ┌──────────────┐ │
│ │U2F Token    │ │    │ │U2F Server    │ │
│ │(USB/NFC)    │ │    │ │Component     │ │
│ └─────────────┘ │    │ └──────────────┘ │
└─────────────────┘    └──────────────────┘
```

### U2F Protocol Flow

#### **Registration**
```
1. User enters username/password
2. Server generates registration challenge
3. Browser sends challenge to U2F token
4. User presses token button (user presence test)
5. Token generates key pair for this origin
6. Public key and attestation returned to server
7. Server stores public key for user account
```

#### **Authentication**
```
1. User enters username/password
2. Server sends authentication challenge + key handle
3. Browser forwards to U2F token
4. Token verifies key handle and origin
5. User presses token button
6. Token signs challenge with private key
7. Server validates signature with stored public key
```

### Security Features

#### **Origin Binding**
- **Application ID (AppID)**: Cryptographically tied to web origin
- **Protection**: Prevents phishing sites from using tokens
- **Implementation**: Token verifies AppID matches registered origin

#### **Attestation**
- **Device verification**: Cryptographic proof of token authenticity
- **Batch attestation**: Privacy-preserving device validation
- **Revocation support**: Ability to blacklist compromised token models

#### **Counter Mechanism**
- **Clone detection**: Monotonic counter prevents token duplication
- **Usage tracking**: Server monitors for unusual counter patterns
- **Security enhancement**: Additional protection against physical attacks

---

## FIDO2 and WebAuthn

### Evolution to FIDO2
**FIDO2** = Next-generation protocol combining UAF and U2F capabilities

### WebAuthn Standard
- **W3C Specification**: Web Authentication API standardized by W3C
- **Browser Integration**: Native browser support without plugins
- **Platform Support**: Windows Hello, Touch ID, Android Biometrics

### WebAuthn API Structure

#### **Registration (navigator.credentials.create)**
```javascript
const credential = await navigator.credentials.create({
    publicKey: {
        challenge: new Uint8Array(32),
        rp: { name: "Example Corp" },
        user: {
            id: new TextEncoder().encode("user123"),
            name: "john@example.com",
            displayName: "John Doe"
        },
        pubKeyCredParams: [{ alg: -7, type: "public-key" }],
        authenticatorSelection: {
            authenticatorAttachment: "platform", // or "cross-platform"
            userVerification: "required"
        }
    }
});
```

#### **Authentication (navigator.credentials.get)**
```javascript
const assertion = await navigator.credentials.get({
    publicKey: {
        challenge: new Uint8Array(32),
        allowCredentials: [{
            id: credentialId,
            type: "public-key"
        }],
        userVerification: "required"
    }
});
```

### Platform vs Cross-Platform Authenticators

#### **Platform Authenticators**
- **Integration**: Built into device (Touch ID, Windows Hello, Android)
- **Advantages**: Seamless UX, no additional hardware
- **Limitations**: Device-specific, not portable

#### **Cross-Platform Authenticators**
- **Portability**: USB, NFC, Bluetooth security keys
- **Advantages**: Works across devices, dedicated security hardware
- **Considerations**: Additional cost, potential for loss

---

## Federation Integration and Enterprise Deployment

### FIDO and Federation Protocols

#### **Integration Architecture**
```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   User Agent    │    │Identity Provider │    │Relying Party    │
│                 │    │                  │    │                 │
│ ┌─────────────┐ │    │ ┌──────────────┐ │    │ ┌─────────────┐ │
│ │FIDO Client  │◄┼────┼►│FIDO Server   │◄┼────┼►│SAML/OIDC    │ │
│ │             │ │    │ │              │ │    │ │Relying Party│ │
│ └─────────────┘ │    │ └──────────────┘ │    │ └─────────────┘ │
│        │        │    │        │         │    └─────────────────┘
│ ┌─────────────┐ │    │ ┌──────────────┐ │
│ │Authenticator│ │    │ │Identity      │ │
│ │             │ │    │ │Provider      │ │
│ └─────────────┘ │    │ └──────────────┘ │
└─────────────────┘    └──────────────────┘
```

#### **SAML Integration**
- **AuthnContextClassRef**: Specify FIDO authentication in SAML requests
- **Authentication Context**: Return FIDO-specific context in assertions
- **Step-up Authentication**: Request stronger authentication based on risk

```xml
<!-- SAML Request -->
<samlp:RequestedAuthnContext Comparison="exact">
    <saml:AuthnContextClassRef>
        urn:rsa:names:tc:SAML:2.0:ac:classes:FIDO
    </saml:AuthnContextClassRef>
</samlp:RequestedAuthnContext>

<!-- SAML Response -->
<saml:AuthnContextClassRef>
    urn:rsa:names:tc:SAML:2.0:ac:classes:FIDO
</saml:AuthnContextClassRef>
```

#### **OpenID Connect Integration**
- **acr_values**: Request FIDO authentication in OIDC
- **amr claim**: Return authentication method reference
- **Step-up flow**: Dynamic authentication requirements

```json
// OIDC Request
{
  "acr_values": "phrh mfa fido",
  "scope": "openid profile"
}

// ID Token Response
{
  "acr": "phrh",
  "amr": ["hwk", "bio"],
  "auth_time": 1490898779
}
```

### Enterprise Deployment Strategies

#### **Deployment Models**

**1. Primary Authentication**
- **Use case**: Replace passwords entirely with FIDO
- **Benefits**: Eliminates password-related attacks
- **Considerations**: Requires comprehensive device enrollment

**2. Step-up Authentication**
- **Use case**: Require FIDO for high-risk transactions
- **Benefits**: Risk-based security enhancement
- **Implementation**: Policy-driven authentication escalation

**3. Dual Authentication**
- **Use case**: Two different FIDO keys for maximum security
- **Benefits**: Meets highest assurance requirements (e.g., FAL3)
- **Example**: Different keys for authentication and transaction signing

#### **Organizational Benefits**
- **Reduced help desk calls**: Fewer password-related support requests
- **Improved productivity**: Faster authentication flows
- **Enhanced security**: Protection against phishing and credential theft
- **Cost optimization**: Lower total cost of ownership vs traditional 2FA

#### **Deployment Considerations**
- **Device distribution**: Centralized vs user-managed enrollment
- **Backup mechanisms**: Recovery procedures for lost authenticators
- **Policy management**: Authentication requirements by application/risk
- **User training**: Education on FIDO usage and security benefits

---

## Privacy Principles and Security Architecture

### Privacy by Design

#### **Core Privacy Principles**
1. **No global identifiers**: Unique keys per relying party prevent tracking
2. **Biometric data protection**: Biometrics never leave the device
3. **Minimal data collection**: Only necessary authentication data collected
4. **User consent**: Explicit approval for each relying party registration
5. **Unlinkability**: Cannot correlate user activity across services

#### **Privacy Implementation**
```
User Registration Process:
1. Generate unique key pair per (User, Device, Relying Party)
2. Store biometric templates locally only
3. Share only public key and attestation with server
4. Use batch attestation to prevent device tracking
5. Require user consent for each new registration
```

#### **Attestation Privacy**
- **Batch attestation**: Same attestation key for 100,000+ devices
- **Anonymous credentials**: Advanced cryptographic techniques (ECDAA)
- **Privacy CA**: Trusted third-party attestation validation
- **Metadata service**: Centralized authenticator information without tracking

### Security Architecture

#### **Cryptographic Foundations**
- **ECDSA over NIST P-256**: Primary signature algorithm
- **SHA-256**: Hashing for all operations
- **Strong random number generation**: Critical for key security
- **Hardware security modules**: Preferred key storage

#### **Attack Resistance**

**AC1 - Scalable Remote Attacks**
- **Protection**: Public key cryptography prevents credential theft
- **Mitigation**: No shared secrets stored on servers

**AC2 - Scalable Device Attacks** 
- **Protection**: Hardware-protected private keys
- **Mitigation**: Secure elements, TEEs, biometric verification

**AC3 - Per-Session Attacks**
- **Protection**: User presence testing, origin binding
- **Mitigation**: Challenge-response with user interaction

**AC4 - Session Attacks**
- **Protection**: Transaction confirmation, secure displays
- **Mitigation**: WYSIWYS for high-value operations

**AC5/AC6 - Physical Attacks**
- **Protection**: Tamper-resistant hardware, fault injection resistance
- **Mitigation**: Side-channel attack resistance, secure key storage

#### **Threat Mitigation Matrix**

| Threat | FIDO Protection | Implementation |
|--------|----------------|----------------|
| Password reuse | Unique keys per RP | Per-RP key generation |
| Phishing | Origin binding | AppID verification |
| MITM attacks | Channel binding | TLS Channel ID |
| Credential theft | Public key storage | No server secrets |
| Device cloning | Counter mechanism | Monotonic signatures |
| Biometric theft | Local processing | No biometric transmission |

---

## Implementation Standards and Specifications

### FIDO Alliance Specifications

#### **Core Protocols**
- **FIDO UAF Protocol v1.2**: Passwordless authentication
- **FIDO U2F Protocol v1.2**: Second-factor authentication  
- **FIDO2/WebAuthn**: Modern web authentication standard
- **Client to Authenticator Protocol v2.0 (CTAP2)**: Cross-platform communication

#### **Supporting Standards**
- **Authenticator Metadata**: Device capability and security descriptions
- **Conformance Testing**: Certification requirements and test suites
- **Security Reference**: Threat analysis and security measures
- **Implementation Guidelines**: Best practices for deployment

### Certification Program

#### **FIDO Ready™ Program**
- **Server certification**: FIDO UAF/U2F server implementations
- **Authenticator certification**: Hardware and software authenticators
- **Client certification**: Browser and application implementations
- **Interoperability testing**: Cross-vendor compatibility validation

#### **Certification Levels**
- **Level 1**: Basic functional compliance
- **Level 2**: Enhanced security requirements
- **Level 3**: Highest assurance with hardware protection
- **FIPS 140-2**: Government-grade security certification

---

## Market Adoption and Real-World Deployments

### Major Deployments

#### **Consumer Services**
- **Google**: Chrome browser, Google accounts (50M+ users)
- **Facebook**: Account security for high-risk users
- **Dropbox**: U2F security keys for account protection
- **GitHub**: Developer account security
- **Twitter**: Account protection for verified users

#### **Enterprise Implementations**
- **Microsoft**: Windows Hello, Azure AD integration
- **AWS**: IAM security keys for console access
- **Salesforce**: Enterprise SSO with FIDO authentication
- **Financial services**: Online banking transaction confirmation

#### **Government Adoption**
- **UK Government**: GOV.UK Verify with YubiKey integration
- **US Government**: NIST recommendations for federal agencies
- **Estonia**: Integration with national eID infrastructure

### Market Evolution

#### **Device Ecosystem**
- **Hardware tokens**: YubiKey, HyperFIDO, Feitian keys
- **Mobile platforms**: Android, iOS biometric integration
- **Desktop platforms**: Windows Hello, macOS Touch ID
- **Embedded systems**: IoT device authentication

#### **Browser Support**
- **Chrome**: Full FIDO2/WebAuthn support since v67
- **Firefox**: WebAuthn support since v60
- **Safari**: WebAuthn support since v14
- **Edge**: Native Windows Hello integration

---

## Future Directions and Advanced Topics

### Emerging Standards

#### **FIDO Device Onboard (FDO)**
- **Purpose**: Secure IoT device provisioning
- **Benefit**: Zero-touch device deployment
- **Use case**: Industrial IoT, smart city infrastructure

#### **WebAuthn Level 2**
- **Enhancements**: Improved credential management
- **Features**: Conditional mediation, enterprise attestation
- **Timeline**: Ongoing W3C standardization

### Advanced Deployment Patterns

#### **Passwordless Enterprise**
- **Vision**: Complete elimination of passwords
- **Requirements**: Comprehensive FIDO deployment
- **Challenges**: Legacy system integration, user adoption

#### **Zero Trust Architecture**
- **Integration**: FIDO as primary authentication
- **Benefits**: Strong device identity, continuous verification
- **Implementation**: Risk-based authentication policies

#### **Decentralized Identity**
- **Convergence**: FIDO + blockchain identity solutions
- **Potential**: Self-sovereign identity management
- **Research**: W3C DID working group collaboration

---

## Key Concepts Summary

### Critical Terms
- **UAF vs U2F**: Passwordless vs second-factor authentication
- **Roaming vs Platform**: Cross-device vs device-bound authenticators  
- **Attestation vs Assertion**: Device identity vs user authentication
- **AppID vs Origin**: Application identifier vs web origin
- **TEE vs Secure Element**: Trusted execution vs dedicated hardware

### Design Principles
1. **User-centric design**: Prioritize usability without compromising security
2. **Privacy by design**: Protect user information through technical measures
3. **Cryptographic protection**: Use strong, proven cryptographic primitives
4. **Open standards**: Enable interoperability across vendors and platforms
5. **Future-proof architecture**: Support evolution of authentication technologies

### Real-World Applications
- **Enterprise SSO**: Active Directory, SAML, OIDC integration
- **Consumer web services**: Social media, email, cloud storage
- **Financial services**: Online banking, payment card authentication
- **Government services**: Digital identity, secure document access
- **IoT and embedded**: Device authentication, secure provisioning

---

## Practice Questions

### Conceptual Understanding:
1. Explain how FIDO UAF achieves passwordless authentication while maintaining security.
2. Compare FIDO U2F and UAF protocols in terms of use cases and security properties.
3. How does FIDO's privacy design prevent user tracking across different services?

### Applied Analysis:
1. Design a FIDO deployment strategy for a large enterprise with 10,000 employees.
2. Analyze the integration between FIDO authentication and SAML-based federation.
3. How would you implement step-up authentication using FIDO for high-value transactions?

### Critical Thinking:
1. What are the implications of quantum computing for FIDO's cryptographic foundations?
2. How might FIDO evolve to support emerging technologies like augmented reality interfaces?
3. What challenges must be addressed for FIDO adoption in developing countries with limited smartphone penetration?

### Security Assessment:
1. Evaluate FIDO's resistance to various attack classes (AC1-AC6) from the security reference.
2. How do different authenticator assurance levels impact overall system security?
3. What are the trade-offs between convenience and security in platform vs cross-platform authenticators?
