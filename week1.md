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

---

## Practice Questions

### Conceptual Understanding:

<div class="question-item">
<div class="question">
1. Explain why authentication is considered the foundation of security systems.
<button class="toggle-btn" onclick="toggleAnswer(this)">Show Answer</button>
</div>
<div class="answer">
Authentication forms the foundation because it establishes trust in digital systems. Without reliable authentication, authorization becomes meaningless - you can't grant appropriate permissions if you don't know who is requesting access. It prevents unauthorized access, enables accountability through audit trails, and supports non-repudiation. In the AAA model, authentication must come first as it identifies the entity before determining what they can do (authorization) or tracking what they did (accounting).
</div>
</div>

<div class="question-item">
<div class="question">
2. Compare the security and usability trade-offs of the four authentication factors.
<button class="toggle-btn" onclick="toggleAnswer(this)">Show Answer</button>
</div>
<div class="answer">
<strong>Knowledge factors</strong> are highly usable (familiar, easily changed) but vulnerable to sharing and guessing. <strong>Possession factors</strong> offer better security through physical tokens but can be lost or stolen. <strong>Inherence factors</strong> (biometrics) provide strong security and convenience but raise privacy concerns and can't be changed if compromised. <strong>Context factors</strong> are transparent to users but complex to implement and can change legitimately. The optimal approach combines multiple factors to balance security with user experience.
</div>
</div>

<div class="question-item">
<div class="question">
3. Why is salt essential in password storage, and how does it prevent rainbow table attacks?
<button class="toggle-btn" onclick="toggleAnswer(this)">Show Answer</button>
</div>
<div class="answer">
Salt prevents rainbow table attacks by ensuring each password hash is unique, even for identical passwords. Rainbow tables contain precomputed hashes for common passwords, but when salt is added, attackers would need separate rainbow tables for every possible salt value, making the attack computationally infeasible. Salt forces attackers to compute hashes individually for each password, dramatically increasing the time and resources required for brute force attacks.
</div>
</div>

### Applied Analysis:

<div class="question-item">
<div class="question">
1. Design an authentication system for a banking application. What factors would you use and why?
<button class="toggle-btn" onclick="toggleAnswer(this)">Show Answer</button>
</div>
<div class="answer">
For banking, I'd implement: <strong>Knowledge factor</strong> (strong password/PIN), <strong>Possession factor</strong> (mobile app with push notifications or SMS), <strong>Context factors</strong> (device fingerprinting, geolocation, behavioral patterns). For high-value transactions, add <strong>Inherence factor</strong> (biometrics). This provides multiple layers of security while maintaining usability. Include risk-based authentication that requires additional factors for unusual activity patterns, locations, or transaction amounts.
</div>
</div>

<div class="question-item">
<div class="question">
2. Analyze a password policy: length 8+, mixed case, numbers, symbols, 90-day expiration. What are the strengths and weaknesses?
<button class="toggle-btn" onclick="toggleAnswer(this)">Show Answer</button>
</div>
<div class="answer">
<strong>Strengths</strong>: Enforces complexity that resists brute force attacks, prevents long-term credential reuse. <strong>Weaknesses</strong>: 8 characters is relatively short by modern standards, complexity requirements often lead to predictable patterns (Password1!, Password2!), 90-day expiration causes user fatigue and encourages weak password practices like incrementing numbers. <strong>Better approach</strong>: Longer minimum length (12+ characters), encourage passphrases, implement breach monitoring instead of arbitrary expiration, focus on password uniqueness across services.
</div>
</div>

<div class="question-item">
<div class="question">
3. How would you implement secure password reset for users who forget their credentials?
<button class="toggle-btn" onclick="toggleAnswer(this)">Show Answer</button>
</div>
<div class="answer">
Multi-step process: <strong>Identity verification</strong> through multiple channels (email + SMS, or security questions + email), <strong>Time-limited tokens</strong> (15-30 minutes), <strong>Single-use reset links</strong> that expire after use, <strong>Account lockout</strong> after multiple failed attempts, <strong>Audit logging</strong> of all reset attempts, <strong>Alternative verification</strong> methods for compromised email accounts (in-person verification, secondary email, trusted contacts). Never send passwords in plaintext; always require users to create new passwords through secure forms.
</div>
</div>

### Critical Thinking:

<div class="question-item">
<div class="question">
1. How might quantum computing impact current password hashing techniques?
<button class="toggle-btn" onclick="toggleAnswer(this)">Show Answer</button>
</div>
<div class="answer">
Quantum computers using Grover's algorithm could theoretically reduce the effective security of hash functions by half (256-bit becomes 128-bit equivalent). However, current password hashing functions like Argon2, scrypt, and bcrypt derive their security more from computational cost and memory requirements than pure cryptographic strength. The iterative nature and memory-hard functions would still require significant quantum resources. Organizations should plan for post-quantum cryptography and consider increasing iteration counts and key lengths as quantum computing advances.
</div>
</div>

<div class="question-item">
<div class="question">
2. What authentication challenges emerge in IoT devices with limited user interfaces?
<button class="toggle-btn" onclick="toggleAnswer(this)">Show Answer</button>
</div>
<div class="answer">
IoT devices face unique challenges: <strong>Limited input methods</strong> (no keyboard/screen), <strong>Constrained resources</strong> (processing power, memory), <strong>Physical access</strong> risks, <strong>Scalability</strong> for device management. Solutions include: <strong>Device certificates</strong> embedded during manufacturing, <strong>Bluetooth/WiFi pairing</strong> with authenticated mobile apps, <strong>QR codes</strong> for initial setup, <strong>Hardware security modules</strong>, <strong>Network-level authentication</strong> through gateways, <strong>Behavioral authentication</strong> based on device usage patterns, <strong>Remote attestation</strong> to verify device integrity.
</div>
</div>

<div class="question-item">
<div class="question">
3. How do cultural differences affect global authentication system design?
<button class="toggle-btn" onclick="toggleAnswer(this)">Show Answer</button>
</div>
<div class="answer">
Cultural factors include: <strong>Privacy expectations</strong> (biometric acceptance varies significantly), <strong>Technology adoption</strong> patterns (mobile-first vs desktop-first regions), <strong>Language and character sets</strong> (password complexity varies by script), <strong>Regulatory requirements</strong> (GDPR in Europe, different data residency laws), <strong>Trust in institutions</strong> affects acceptance of government-issued digital IDs, <strong>Social structures</strong> influence security question effectiveness, <strong>Economic factors</strong> affect device availability and internet connectivity. Global systems need flexible authentication options, localized user experiences, and compliance with regional regulations while maintaining security standards.
</div>
</div>

<style>
.question-item {
    margin-bottom: 20px;
    border: 1px solid #e0e0e0;
    border-radius: 8px;
    overflow: hidden;
}

.question {
    padding: 15px;
    background-color: #f8f9fa;
    font-weight: 600;
}

.answer {
    padding: 15px;
    background-color: #ffffff;
    border-top: 1px solid #e0e0e0;
    display: none;
}

.answer.show {
    display: block;
}

.answer strong {
    font-weight: 700;
    color: #2c3e50;
}

.toggle-btn {
    background-color: #007bff;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
    margin-top: 10px;
    font-size: 14px;
}

.toggle-btn:hover {
    background-color: #0056b3;
}

.toggle-btn.active {
    background-color: #dc3545;
}
</style>

<script>
function toggleAnswer(button) {
    const questionItem = button.closest('.question-item');
    const answer = questionItem.querySelector('.answer');
    
    if (answer.classList.contains('show')) {
        answer.classList.remove('show');
        button.textContent = 'Show Answer';
        button.classList.remove('active');
    } else {
        answer.classList.add('show');
        button.textContent = 'Hide Answer';
        button.classList.add('active');
    }
}
</script>

<div style="display: flex; justify-content: space-between; align-items: center; margin: 30px 0;">
    <button class="nav-btn prev-btn" disabled>
        ← Previous Week
    </button>
    <button class="nav-btn next-btn" onclick="window.location.href='week2.html'">
        Next Week: Biometric Authentication →
    </button>
</div>
```
---

