# Week 1 Study Guide: Authentication - Fundamentals

## Course Context: Security & Data Systems

**Week 1 Focus:** Authentication - Fundamentals  
**Next:** Biometric → Tokens and Two-Factor → FIDO Alliance

---

## Learning Objectives

By the end of Week 1, you should master:
- Core authentication concepts and terminology
- Four authentication factors and their security trade-offs
- Password-based authentication mechanisms and vulnerabilities
- Cryptographic foundations for secure authentication
- User behavior and authentication usability challenges

---

## Authentication Fundamentals

### Definition & Purpose
**Authentication** = The process of verifying that an entity is who they claim to be

### Foundation of Security Architecture
```
Authentication → Authorization → Accounting (AAA Model)
     ↓              ↓              ↓
   Who are you?  What can you do?  What did you do?
```

### Two-Phase Authentication Process

#### **Phase 1: Identification**
- Entity **claims** an identity
- Provides **identifier** (username, email, ID number)
- System **recognizes** the claimed identity

#### **Phase 2: Verification** 
- Entity **proves** they are who they claim to be
- Provides **credentials** matching the claimed identity
- System **validates** the proof

### Security Properties
- **Uniqueness**: Each identity maps to one entity
- **Non-repudiation**: Cannot deny authenticated actions
- **Freshness**: Prevents replay attacks
- **Integrity**: Credentials cannot be modified in transit

---

## Four Authentication Factors

### 1. Knowledge Factors (Something You Know)

#### **Examples:**
- Passwords and passphrases
- PINs and security codes
- Security questions
- Cryptographic keys (memorized)

#### **Characteristics:**
- **Advantages**: Low cost, familiar, easily changed
- **Disadvantages**: Can be forgotten, shared, guessed, observed
- **Security**: Vulnerable to brute force, dictionary attacks, social engineering

#### **Implementation Considerations:**
- **Complexity requirements** vs **usability**
- **Storage security** (hashing, salting)
- **Transmission protection** (encryption)
- **Policy enforcement** (length, character sets, expiration)

### 2. Possession Factors (Something You Have)

#### **Examples:**
- Hardware tokens (RSA SecurID, YubiKey)
- Smart cards and badges
- Mobile phones (SMS, apps)
- Certificates on devices

#### **Characteristics:**
- **Advantages**: Harder to duplicate, can be time-limited
- **Disadvantages**: Can be lost/stolen, requires distribution
- **Security**: Physical theft, cloning, interception

#### **Token Types:**
- **Static**: Fixed passwords/codes
- **Dynamic**: Time-based (TOTP) or counter-based (HOTP)
- **Challenge-response**: Cryptographic computation

### 3. Inherence Factors (Something You Are)

#### **Static Biometrics:**
- Fingerprints, palm prints
- Iris and retina patterns
- Facial geometry
- DNA sequences

#### **Dynamic Biometrics:**
- Voice patterns and speech
- Handwriting dynamics
- Keystroke patterns
- Gait analysis

#### **Characteristics:**
- **Advantages**: Unique, cannot be forgotten, difficult to transfer
- **Disadvantages**: Privacy concerns, permanent if compromised, can change
- **Security**: Spoofing, template theft, false accept/reject rates

### 4. Context Factors (Something About You)

#### **Examples:**
- Location (GPS, IP geolocation)
- Time patterns (usual login hours)
- Device characteristics (browser fingerprint)
- Behavioral patterns (typing speed, mouse movement)

#### **Characteristics:**
- **Advantages**: Transparent to user, difficult to replicate
- **Disadvantages**: Can change legitimately, complex to implement
- **Security**: Spoofing location, device cloning

---

## Password-Based Authentication Deep Dive

### Password Authentication Workflow

```
1. User enters username + password
2. System retrieves stored password data
3. System verifies password match
4. System grants/denies access based on verification
```

### Password Vulnerabilities & Attack Vectors

#### **Offline Attacks:**
- **Dictionary attacks**: Try common passwords from lists
- **Brute force**: Systematic enumeration of possibilities
- **Rainbow tables**: Precomputed hash lookups
- **Hybrid attacks**: Dictionary + rule-based modifications

#### **Online Attacks:**
- **Credential stuffing**: Reuse of breached credentials
- **Password spraying**: Few passwords against many accounts
- **Targeted guessing**: Personal information-based attempts

#### **Side-Channel Attacks:**
- **Shoulder surfing**: Visual observation
- **Keylogging**: Capture keystrokes
- **Timing attacks**: Analyze response timing
- **Acoustic attacks**: Sound analysis

#### **Social Engineering:**
- **Phishing**: Fraudulent credential collection
- **Pretexting**: Impersonation to extract passwords
- **Baiting**: Malware-infected media

### Password Security Mechanisms

#### **Cryptographic Hashing**
```
Instead of: store("alice", "password123")
Do this:   store("alice", hash("password123" + salt))
```

**Why Hashing?**
- **One-way function**: Easy to compute, hard to reverse
- **Deterministic**: Same input → same output
- **Avalanche effect**: Small input change → large output change

#### **Salt Implementation**
```
password = "secret123"
salt = random_bytes(16)  // 128-bit random value
stored_hash = SHA256(password + salt)
store(username, salt + stored_hash)
```

**Salt Benefits:**
- **Prevents rainbow table attacks**
- **Forces unique computation per password**
- **Increases attack complexity exponentially**

#### **Advanced Techniques:**
- **Key stretching**: Multiple hash iterations (PBKDF2, bcrypt, scrypt)
- **Pepper**: Secret salt stored separately from database
- **Adaptive hashing**: Increase complexity over time

### Password Policy Design

#### **Complexity Requirements:**
- **Length**: Minimum 8-12 characters (longer is better)
- **Character sets**: Uppercase, lowercase, digits, symbols
- **Patterns**: Avoid keyboard walks, repeated characters
- **Dictionary**: Block common passwords and variations

#### **Lifecycle Management:**
- **Expiration**: Balance security vs usability (90-365 days)
- **History**: Prevent reuse of recent passwords
- **Reset procedures**: Secure recovery mechanisms
- **Emergency access**: Backup authentication methods

---

## User Behavior & Authentication Usability

### Human Factors in Authentication

#### **Password Creation Behavior:**
- Users choose **predictable patterns**
- **Personal information** heavily influences choices
- **Convenience over security** in most decisions
- **Compliance fatigue** with complex requirements

#### **Password Management Challenges:**
- **Memory limitations**: Cannot remember many complex passwords
- **Multiple accounts**: Average user has 100+ accounts
- **Reuse patterns**: Same password across multiple services
- **Weak backup**: Security questions often guessable

### Research Findings

#### **Password Strength vs Usability:**
- **Length** more important than complexity for entropy
- **Passphrases** better than complex character requirements
- **Visual feedback** helps users create stronger passwords
- **Breach notifications** motivate password changes

#### **Multi-Factor Authentication Adoption:**
- **Security professionals**: 95% adoption
- **General users**: 28% adoption
- **Primary barrier**: Perceived inconvenience
- **Driver**: Experience with account compromise

### Design Principles for Usable Security

#### **Minimize User Burden:**
- **Single sign-on (SSO)** where possible
- **Password managers** integration
- **Biometric shortcuts** for frequent access
- **Risk-based authentication** (step-up when needed)

#### **Clear Security Communication:**
- **Visual indicators** of password strength
- **Educational tooltips** about security practices
- **Breach notifications** with clear action steps
- **Security dashboard** showing account status

---

## Cryptographic Foundations

### Hash Functions in Authentication

#### **Properties Required:**
- **Deterministic**: Same input → same output
- **Uniform distribution**: Output evenly distributed
- **Avalanche effect**: Small change → large output change
- **Irreversible**: Computationally infeasible to reverse

#### **Common Hash Functions:**
- **MD5**: 128-bit, deprecated (collision vulnerabilities)
- **SHA-1**: 160-bit, deprecated (collision found 2017)
- **SHA-2**: 224/256/384/512-bit, currently recommended
- **SHA-3**: Latest standard, different construction

#### **Password-Specific Functions:**
- **PBKDF2**: NIST standard, configurable iterations
- **bcrypt**: Adaptive cost parameter
- **scrypt**: Memory-hard function
- **Argon2**: Winner of password hashing competition

### Key Derivation & Storage

#### **Key Stretching Process:**
```
1. Start with password + salt
2. Apply hash function repeatedly (1000-100000 iterations)
3. Output becomes derived key
4. Store salt + iteration count + derived key
```

#### **Security Benefits:**
- **Slows down brute force attacks**
- **Adapts to hardware improvements**
- **Forces attacker resource commitment**

---

## Authentication in Distributed Systems

### Distributed Authentication Challenges

#### **Network Security:**
- **Eavesdropping**: Credentials intercepted in transit
- **Man-in-the-middle**: Attacker between client and server
- **Replay attacks**: Captured credentials reused
- **Session hijacking**: Active session takeover

#### **Scalability Issues:**
- **Single point of failure**: Central authentication server
- **Performance bottlenecks**: All authentication through one service
- **Geographic distribution**: Latency affects user experience
- **Load balancing**: Consistent authentication across servers

#### **Trust Relationships:**
- **Cross-domain authentication**: Trusting external identity providers
- **Federation**: Sharing authentication across organizations
- **Certificate authorities**: Trust chain validation
- **Revocation**: Invalidating compromised credentials

### Authentication Protocols

#### **Challenge-Response:**
```
1. Client requests access
2. Server sends random challenge
3. Client computes response using secret
4. Server verifies response
```

#### **Time-Based Protocols:**
- **Kerberos**: Ticket-granting tickets with time limits
- **SAML**: Time-bounded assertions
- **JWT**: Token expiration timestamps

---

## Key Concepts Summary

### Critical Terms
- **Authentication vs Authorization**: Identity verification vs permission granting
- **Factor types**: Knowledge, possession, inherence, context
- **Hash vs encryption**: One-way vs two-way transformations
- **Salt vs pepper**: Public vs secret random values
- **False positive/negative**: Incorrect accept/reject in authentication

### Design Principles
1. **Defense in depth**: Multiple authentication layers
2. **Fail securely**: Default to deny access
3. **Minimize attack surface**: Reduce authentication touchpoints
4. **User-centered design**: Balance security with usability
5. **Continuous monitoring**: Detect authentication anomalies

### Real-World Applications
- **Enterprise SSO**: Active Directory, LDAP
- **Web authentication**: OAuth 2.0, OpenID Connect
- **Mobile apps**: Biometric integration, device binding
- **Financial services**: Multi-factor requirements, fraud detection
