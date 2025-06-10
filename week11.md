# Week 11 Study Guide: Digital Identity Management

## Course Context: Security & Data Systems

**Week 11 Focus:** Digital Identity Management  
**Previous:** Digital Rights Management → **Current:** Identity Systems & SSI → Next: Course Conclusion

---

## Learning Objectives

By the end of Week 11, you should master:
- Kim Cameron's Seven Laws of Identity and their architectural implications
- Digital identity models: Centralized, Federated, and Self-Sovereign Identity (SSI)
- Core IAA protocols: SAML, OAuth 2.0, and OpenID Connect
- SSI components: DIDs, Verifiable Credentials, and Digital Wallets
- European Union Digital Identity (EUDI) architecture and eIDAS 2.0 governance
- Real-world applications and future roadmap for digital identity systems

---

## Digital Identity Fundamentals

### Definition & Core Concepts
**Digital Identity** = A set of claims made by one digital subject about itself or another digital subject

**Digital Subject** = Any person, organization, application, or device that can be referenced in a digital context

### Key Components
- **Identity Manager (IdM)**: Service responsible for identity lifecycle management
- **Identity Provider (IdP)**: Issues identity attributes and assertions for authorized services
- **Relying Party (RP)**: Service that accepts and validates identity claims
- **Claims**: Statements attesting to the truth of identity attribute associations
- **Credentials**: Collections of claims affirmed by an issuer about one or more subjects

### Identity Lifecycle Management
```
Provisioning → Propagation → Usage → Maintenance → Deletion
```

---

## Kim Cameron's Seven Laws of Identity

### 1. User Control and Consent
**Technical identity systems must only reveal information identifying a user with the user's consent.**

#### Key Principles:
- **User-centric design**: Put users in control of identity disclosure
- **Explicit consent**: Clear agreement before information sharing
- **Transparency**: Users must understand what information is collected and why
- **Deception protection**: Verify identity of parties requesting information
- **Consistent experience**: Maintain user control across all contexts

### 2. Minimal Disclosure
**The solution that discloses the least amount of identifying information and best limits its use is the most stable long-term solution.**

#### Implementation:
- **Need-to-know basis**: Acquire only necessary information
- **Age categories vs birth dates**: Use less identifying alternatives
- **Context-specific identifiers**: Avoid cross-context unique identifiers
- **Risk minimization**: Aggregation of identifying information aggregates risk

### 3. Justifiable Parties
**Digital identity systems must be designed so the disclosure of identifying information is limited to parties having a necessary and justifiable place in a given identity relationship.**

#### Requirements:
- **Clear party identification**: Users must know who they're sharing with
- **Justified access**: Both subjects and relying parties must justify information needs
- **Transparent relationships**: No hidden intermediaries in identity transactions

### 4. Directed Identity
**A universal identity system must support both "omnidirectional" identifiers for use by public entities and "unidirectional" identifiers for use by private entities.**

#### Application:
- **Public entities**: Government services can use omnidirectional identifiers
- **Private entities**: Should use unidirectional identifiers for privacy
- **Context appropriateness**: Match identifier type to use case

### 5. Pluralism of Operators and Technologies
**A universal identity system must channel and enable the inter-working of multiple identity technologies run by multiple identity providers.**

#### Rationale:
- **Context diversity**: Different scenarios require different identity solutions
- **No monolithic system**: Single systems cannot meet all requirements
- **Metasystem approach**: Framework enabling interoperability between systems
- **Technology choice**: Support for various authentication methods and providers

### 6. Human Integration
**The universal identity metasystem must define the human user to be a component of the distributed system integrated through unambiguous human-machine communication mechanisms offering protection against identity attacks.**

#### Design Requirements:
- **Predictable ceremonies**: Predetermined interaction patterns
- **Protection against phishing**: Secure human-computer communication channel
- **Clear visual indicators**: Unambiguous identity information presentation
- **Cross-platform safety**: Security that doesn't rely on platform obscurity

### 7. Consistent Experience Across Contexts
**The unifying identity metasystem must guarantee its users a simple, consistent experience while enabling separation of contexts through multiple operators and technologies.**

#### Implementation:
- **"Thingification" of identities**: Visual representation of digital identities
- **Context awareness**: Different identities for different scenarios
- **Unified interface**: Consistent selection and management experience
- **Clear identity types**: Users understand options and make informed choices

---

## Digital Identity Models

### 1. Centralized Identity Model

#### **Characteristics:**
- **Account-based identity**: Username/password registration with services
- **Organizational control**: Organizations own and control user credentials
- **Direct relationships**: Users authenticate directly to each service
- **Limited portability**: Credentials tied to specific organizations

#### **Examples:**
- **Scandinavian model**: Private companies provide centralized identity for government interaction
- **Continental model**: Governments provide identity services for citizen-company interaction
- **Traditional web accounts**: Facebook, Twitter, bank accounts

#### **Problems:**
- **Password burden**: Users must remember multiple complex passwords
- **Security vulnerabilities**: Centralized databases become attractive targets
- **Limited control**: Users have minimal control over their data
- **Non-portable data**: Information cannot be transferred between services

### 2. Federated Identity Model

#### **Architecture:**
```
User ← Account → Identity Provider (IdP) → Relying Party (RP)
```

#### **Key Features:**
- **Single Sign-On (SSO)**: One authentication for multiple services
- **Trust relationships**: IdPs and RPs establish federated trust
- **Standard protocols**: SAML, OAuth 2.0, OpenID Connect
- **Reduced password fatigue**: Fewer credentials to manage

#### **Examples:**
- **Social login**: "Login with Google/Facebook/Microsoft"
- **Enterprise SSO**: Active Directory federation
- **Government federation**: Autenticação.Gov (Portugal)

#### **Benefits:**
- **Convenience**: Reduced authentication friction
- **Centralized management**: Single point for credential updates
- **Improved security**: Professional identity provider security

### 3. Self-Sovereign Identity (SSI) Model

#### **Philosophy:**
Users should be able to create, control, and manage their own identity in the most autonomous and decentralized way possible, without depending on any authority other than their own.

#### **Core Components:**
- **Digital Wallets**: Secure storage and management of credentials
- **Decentralized Identifiers (DIDs)**: Unique, self-sovereign identifiers
- **Verifiable Credentials (VCs)**: Cryptographically secure identity claims
- **Blockchain/DLT**: Decentralized infrastructure for trust and verification

#### **Control Shift:**
- **Centralized/Federated**: Control with issuers and verifiers
- **SSI**: Control gravitates to the user as peer in the network

---

## SSI Essential Properties (Christopher Allen, 2016)

### Ten Fundamental Properties:

1. **Existence**: Entities have independent existence
2. **Control**: Entities control their identities
3. **Access**: Entities have full access to their own data
4. **Transparency**: Systems and algorithms are known and auditable
5. **Persistence**: Identities are long-lasting
6. **Portability**: Information and services are transportable
7. **Interoperability**: Identities work across wide networks
8. **Consent**: Agreement required for identity use
9. **Minimization**: Disclosure limited to required information
10. **Protection**: Rights of entities must be protected

---

## SSI Technical Architecture

### Decentralized Identifiers (DIDs)

#### **Purpose:**
- **Self-sovereign identifiers**: Created and controlled by identity owner
- **Resolvable**: DIDs resolve to DID documents containing public keys
- **Cryptographic security**: Enable secure, authenticated connections

#### **Structure:**
```
DID → DID Document → Public Key + Service Endpoint
```

### Verifiable Credentials (VCs)

#### **Definition:**
Cryptographically secure, tamper-evident credentials that can be verified without contacting the issuer.

#### **Components:**
- **Issuer**: Entity that creates and signs the credential
- **Subject**: Entity the credential is about
- **Claims**: Assertions about the subject
- **Proof**: Cryptographic evidence of authenticity

### Digital Wallets

#### **Functions:**
- **DID management**: Create, store, and resolve DIDs
- **Credential storage**: Secure repository for VCs
- **Proof generation**: Create presentations from stored credentials
- **Secure communication**: Establish authenticated connections

#### **Architecture:**
```
Agent (Communication) + Wallet (Storage) = Combined SSI Application
```

---

## IAA Protocols

### SAML (Security Assertion Markup Language)

#### **Purpose:**
XML-based standard for exchanging authentication and authorization data between identity providers and service providers.

#### **Use Cases:**
- **Enterprise SSO**: Employee access to multiple applications
- **Government services**: Citizen authentication across agencies
- **Academic federation**: Student access to educational resources

### OAuth 2.0

#### **Purpose:**
Authorization protocol allowing third-party applications to access user resources without exposing passwords.

#### **Flow:**
```
1. Authorization Request → Authorization Code
2. Authorization Code → Access Token
3. Access Token → Resource Access
```

#### **Components:**
- **Resource Owner**: User who owns the data
- **Client**: Application requesting access
- **Authorization Server**: Issues access tokens
- **Resource Server**: Hosts protected resources

### OpenID Connect (OIDC)

#### **Purpose:**
Identity layer built on OAuth 2.0, enabling authentication and basic profile information exchange.

#### **Extension to OAuth:**
- **ID Token**: JWT containing user identity claims
- **UserInfo Endpoint**: Additional user information access
- **Standardized claims**: Common identity attributes

#### **Flow:**
```
Authentication → ID Token + Access Token → Profile Information
```

---

## European Union Digital Identity (EUDI)

### EUDI Architecture Reference Framework

#### **Objective:**
Provide interoperable European Union Digital Identity Wallets (EUDIW) for citizens, residents, and businesses across the European Economic Area (EEE).

#### **Key Features:**
- **Certified mobile apps**: Verified EUDIW applications
- **Interoperability**: Cross-border functionality within EEE
- **Independent development**: Multiple entities can develop certified wallets
- **SSI principles**: User control and privacy by design

### EUDIW Components

#### **Wallet Functionality:**
- **Credential management**: Request, obtain, and store VCs securely
- **Service access**: Authenticate to online services
- **Data presentation**: Present identity information as needed
- **Digital signatures**: Sign and authenticate documents electronically

#### **Trust Infrastructure:**
- **Trusted Issuers Registry**: Managed via EBSI (European Blockchain Services Infrastructure)
- **Qualified trust services**: eIDAS-compliant credential issuance
- **Cross-border recognition**: Mutual recognition across EU member states

---

## eIDAS 2.0 Governance Model

### Regulatory Framework

#### **Purpose:**
Updated eIDAS regulation adapted for decentralized services supported by EBSI, providing legal governance for EUDI implementation.

#### **Timeline:**
- **2025**: Full operational capability
- **2030**: Target of 80% European citizen adoption

### Implementation Roadmap

#### **2021-2023**: Conception and initial pilots
#### **2024**: Formal regulation and technical specifications
#### **2025**: Digital wallet development in EU countries
#### **2026**: Wallet availability to citizens
#### **2027**: Mandatory acceptance by private companies and digital services

### Use Cases

#### **Identity Documents:**
- **Citizen cards**: Digital representation of national ID
- **Driving licenses**: Mobile driving permits
- **Professional diplomas**: Verified educational credentials
- **Medical certificates**: Healthcare documentation
- **Academic degrees**: University and training certifications

#### **Service Categories:**
- **Public sector**: Government service access
- **Banking/Insurance**: Financial service authentication
- **Healthcare**: Medical record access and sharing
- **Education**: Academic credential verification
- **Travel**: Digital passport and visa functionality
- **Commerce**: Age verification and customer onboarding

---

## Security Considerations

### Digital Identity Threats

#### **Identity Theft:**
- **Centralized vulnerabilities**: Single points of failure
- **Credential stuffing**: Reused password attacks
- **Social engineering**: Phishing and pretexting

#### **Privacy Concerns:**
- **Correlation attacks**: Linking activities across services
- **Data aggregation**: Building comprehensive user profiles
- **Surveillance**: Government and corporate monitoring

#### **Technical Vulnerabilities:**
- **Weak authentication**: Password-only systems
- **Session hijacking**: Compromised authentication tokens
- **Man-in-the-middle**: Intercepted communications

### SSI Security Benefits

#### **Decentralization:**
- **Eliminates central honeypots**: No single database to compromise
- **User control**: Individuals manage credential exposure
- **Reduced correlation**: Different identifiers for different contexts

#### **Cryptographic Protection:**
- **Verifiable credentials**: Tamper-evident digital credentials
- **Zero-knowledge proofs**: Prove attributes without revealing values
- **Selective disclosure**: Share only necessary information

---

## Future Implications

### Digital Society Transformation

#### **Citizen Empowerment:**
- **Autonomy**: Individual control over digital identity
- **Privacy**: Minimal disclosure principles
- **Portability**: Freedom to change service providers

#### **Service Innovation:**
- **Frictionless authentication**: Seamless service access
- **Trust infrastructure**: Verified credential ecosystem
- **Interoperability**: Cross-border digital services

### Challenges and Considerations

#### **Adoption Barriers:**
- **Technical complexity**: User experience challenges
- **Infrastructure requirements**: Widespread wallet deployment
- **Regulatory harmonization**: Cross-jurisdictional compliance

#### **Long-term Vision:**
- **Digital-first governance**: Government services via digital identity
- **Economic integration**: Unified European digital market
- **Democratic participation**: Secure digital voting systems

---

## Key Concepts Summary

### Critical Terms
- **Claims vs Credentials**: Individual assertions vs collections of claims
- **DIDs vs Traditional IDs**: Self-sovereign vs authority-issued identifiers
- **SSI vs Federation**: User control vs provider control
- **Verifiable vs Verified**: Cryptographically checkable vs checked credentials
- **Wallet vs Agent**: Storage vs communication components

### Design Principles
1. **User sovereignty**: Individual control over identity
2. **Minimal disclosure**: Share only necessary information
3. **Decentralized trust**: Distributed verification mechanisms
4. **Interoperability**: Cross-system compatibility
5. **Privacy by design**: Built-in privacy protection

### Real-World Applications
- **EUDIW**: European digital wallet initiative
- **Estonia e-Residency**: Digital citizenship program
- **Hyperledger Indy**: SSI platform implementation
- **Microsoft ION**: DID network on Bitcoin blockchain

---

## Practice Questions

### Conceptual Understanding:
1. Explain how Kim Cameron's Laws of Identity address the fundamental challenges of digital identity systems.
2. Compare the control models of centralized, federated, and self-sovereign identity systems.
3. Why is minimal disclosure crucial for long-term identity system stability?

### Applied Analysis:
1. Design an SSI-based student credential system for a university. What components would you need?
2. Analyze the privacy implications of using social login versus SSI for e-commerce authentication.
3. How would you implement selective disclosure for a digital driver's license?

### Critical Thinking:
1. How might quantum computing impact the cryptographic foundations of SSI systems?
2. What governance challenges arise when implementing cross-border digital identity systems?
3. How do cultural differences in privacy expectations affect global SSI adoption?

### EU-Specific Scenarios:
1. How does eIDAS 2.0 balance innovation with regulatory compliance?
2. What interoperability challenges must EUDIW overcome for pan-European adoption?
3. How might GDPR and eIDAS 2.0 interact in SSI implementations?