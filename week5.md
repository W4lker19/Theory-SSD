# Week 5 Study Guide: Access Control - Fundamentals

## Course Context: Security & Data Systems

**Week 5 Focus:** Access Control - Fundamentals  
**Previous:** Authentication, Biometric, Tokens and Two-Factor, FIDO Alliance  
**Related:** Digital Identity Management, Software Security

---

## Learning Objectives

By the end of Week 5, you should master:
- Core access control concepts and terminology
- Access control models: discretionary, mandatory, and role-based
- Access control mechanisms: ACLs, capabilities, and access matrices
- Operating system access controls (Unix, Windows, Linux)
- Multilevel security models (Bell-LaPadula, Biba)
- Covert channels and their mitigation strategies
- Real-world access control implementation challenges

---

## Access Control Fundamentals

### Definition & Purpose
**Access Control** = The process of limiting access to resources of a system only to authorized users, programs, processes, or other systems

### Foundation of Security Architecture
```
Authentication → Authorization → Accounting (AAA Model)
     ↓              ↓              ↓
   Who are you?  What can you do?  What did you do?
```

Access control answers the **second question**: determining what authenticated entities are allowed to do.

### Core Components

#### **Subjects**
- **Active entities** that request access to resources
- Users, processes, programs, systems
- Have associated **clearances** or **privileges**

#### **Objects (Resources)**
- **Passive entities** that contain or receive information
- Files, databases, memory segments, network connections
- Have associated **classifications** or **sensitivity levels**

#### **Access Rights**
- **Specific permissions** defining how subjects can interact with objects
- Common rights: Read (r), Write (w), Execute (x), Delete (d), Append (a)
- **Operations** subjects can perform on objects

### Security Properties
- **Confidentiality**: Information disclosed only to authorized entities
- **Integrity**: Information modified only by authorized entities
- **Availability**: Resources accessible to authorized entities when needed
- **Non-repudiation**: Cannot deny performed actions

---

## Access Control Models

### 1. Discretionary Access Control (DAC)

#### **Definition:**
Access control policy determined by the **resource owner** who has discretion over who can access their resources.

#### **Characteristics:**
- **Owner-based**: Resource owners set access permissions
- **Flexible**: Permissions can be changed by owners
- **Identity-based**: Access decisions based on user identity
- **Transitive**: Permissions can be passed to other users

#### **Examples:**
- Traditional Unix file permissions
- Windows NTFS permissions
- Most commercial operating systems

#### **Advantages:**
- **User-friendly**: Intuitive for resource owners
- **Flexible**: Easy to grant/revoke access
- **Scalable**: Works well in collaborative environments

#### **Disadvantages:**
- **Vulnerable to Trojan horses**: Malicious programs can abuse user permissions
- **Information flow control**: Difficult to prevent unauthorized information leakage
- **Security policy enforcement**: No central control over access decisions

### 2. Mandatory Access Control (MAC)

#### **Definition:**
Access control policy **enforced by the system** based on security levels and classifications, independent of user actions.

#### **Characteristics:**
- **System-enforced**: Access decisions made by security policies, not users
- **Classification-based**: Both subjects and objects have security labels
- **Non-discretionary**: Users cannot change access permissions
- **Information flow control**: Prevents unauthorized information flow

#### **Security Labels:**
- **Hierarchical levels**: Unclassified < Confidential < Secret < Top Secret
- **Categories/Compartments**: Additional access restrictions (e.g., Nuclear, Intelligence)
- **Lattice structure**: Combination of levels and categories

#### **Examples:**
- Military/government systems
- SELinux (Security-Enhanced Linux)
- Windows Vista/7+ integrity levels

#### **Advantages:**
- **Strong security**: Prevents many attack vectors
- **Centralized control**: Consistent policy enforcement
- **Information flow protection**: Controls data leakage

#### **Disadvantages:**
- **Rigid**: Difficult to accommodate real-world workflows
- **Complex administration**: Requires specialized knowledge
- **User resistance**: Can impede legitimate work

### 3. Role-Based Access Control (RBAC)

#### **Definition:**
Access control based on **organizational roles** that users assume, with permissions assigned to roles rather than individual users.

#### **Core Components:**
- **Users**: Individual people or entities
- **Roles**: Job functions or organizational positions
- **Permissions**: Specific access rights to resources
- **Sessions**: User activating subset of assigned roles

#### **RBAC Principles:**
- **Least Privilege**: Users get minimum permissions needed for their role
- **Separation of Duties**: Conflicting responsibilities divided among roles
- **Data Abstraction**: Permissions organized around organizational structure

#### **Role Hierarchy:**
```
Senior Manager
    ↓ (inherits permissions)
Manager
    ↓ (inherits permissions)
Employee
```

#### **Examples:**
- Enterprise systems (SAP, Oracle)
- Database management systems
- Banking applications

#### **Advantages:**
- **Administrative efficiency**: Easier to manage than individual permissions
- **Organizational alignment**: Reflects business structure
- **Scalability**: Works well in large organizations
- **Compliance**: Supports audit and regulatory requirements

#### **Disadvantages:**
- **Role explosion**: Can become complex with many fine-grained roles
- **Static nature**: Difficult to handle dynamic access needs
- **Setup complexity**: Initial role design requires significant effort

---

## Access Control Mechanisms

### Access Control Matrix

#### **Concept:**
A **conceptual model** representing access rights as a matrix with subjects as rows and objects as columns.

```
        File1    File2    File3    Printer
Alice   r,w      r        -        w
Bob     r        r,w      r        w  
Carol   -        r        r,w,x    -
```

#### **Implementation Challenges:**
- **Sparse matrix**: Most cells empty in real systems
- **Size**: Becomes unwieldy with many subjects/objects
- **Dynamic changes**: Difficult to update efficiently

### Access Control Lists (ACLs)

#### **Definition:**
Store access control matrix **by columns** - each object has a list of subjects and their permitted access modes.

#### **Structure:**
```
File: /home/alice/document.txt
ACL:
  alice: read, write
  bob: read
  group_finance: read
  other: none
```

#### **Implementation Examples:**

##### **Unix Traditional Permissions:**
```
rwxrw-r--
├─► Owner: read, write, execute
├─► Group: read, write  
└─► Other: read
```

##### **Unix Extended ACLs:**
```
# getfacl /home/alice/document.txt
user::rw-
user:bob:r--
group::r--
group:finance:rw-
mask::rw-
other::---
```

##### **Windows ACLs:**
- **Access Control Entries (ACEs)**: Individual permission entries
- **Security Descriptors**: Complete security information for objects
- **Inheritance**: Child objects inherit parent permissions
- **Explicit vs Inherited**: Direct assignments vs inherited permissions

#### **Advantages:**
- **Natural for users**: Intuitive "who can access what"
- **Efficient revocation**: Easy to find and remove user access
- **Audit-friendly**: Clear view of object permissions
- **Supports groups**: Can assign permissions to user groups

#### **Disadvantages:**
- **Runtime inefficiency**: Must check ACL on each access
- **User enumeration difficult**: Hard to find all objects a user can access
- **Large ACLs**: Performance degrades with many entries
- **Synchronization issues**: Difficult to maintain consistency across systems

### Capabilities

#### **Definition:**
Store access control matrix **by rows** - each subject has a list of objects they can access and the permitted access modes.

#### **Structure:**
```
Subject: Bob
Capabilities:
  File1: read
  File2: read, write
  Printer: write
  Process_A: execute
```

#### **Properties:**
- **Unforgeable tokens**: Cryptographically protected
- **Transferable**: Can delegate capabilities to others
- **Revocable**: Can be invalidated by issuer
- **Time-limited**: Can have expiration dates

#### **Types:**

##### **Object Capabilities:**
- **Combines designation and authority**: Reference to object includes permission
- **No ambient authority**: No system-wide permissions
- **Principle of least privilege**: Only necessary capabilities granted

##### **Traditional Capabilities:**
- **Separate namespace**: Object names separate from permissions
- **Global namespace**: System-wide naming scheme
- **Access mediation**: System checks capabilities on access

#### **Advantages:**
- **Efficient access checking**: No need to search ACLs
- **Easy delegation**: Can pass capabilities to other subjects
- **Least privilege**: Natural support for minimal permissions
- **Distributed systems**: Work well across network boundaries

#### **Disadvantages:**
- **Revocation complexity**: Difficult to revoke distributed capabilities
- **Storage overhead**: Each subject stores many capabilities
- **Confused deputy problem**: Program may misuse capabilities on behalf of user
- **Implementation complexity**: Requires careful design to prevent forgery

### Capability Myths Debunked

#### **Myth 1: ACLs and Capabilities are Equivalent**
**Reality**: While both can represent the same access matrix, they have fundamental differences:
- **Reference direction**: ACLs point from objects to subjects; capabilities point from subjects to objects
- **Dynamic behavior**: Different rules for authority manipulation
- **Granularity**: Capabilities support fine-grained subjects better

#### **Myth 2: Capabilities Cannot Enforce Confinement**
**Reality**: Capabilities can enforce confinement through:
- **Object capability model**: No designation without authority
- **Membrane patterns**: Controlled interaction boundaries
- **Revocable capabilities**: Can withdraw access when needed

#### **Myth 3: Capabilities Cannot be Revoked**
**Reality**: Several revocation mechanisms exist:
- **Indirect capabilities**: Revoke through intermediary objects
- **Revocation lists**: Centralized capability invalidation
- **Time-based expiration**: Automatic capability timeout

---

## Operating System Access Controls

### Unix/Linux Access Control

#### **Traditional Unix Model:**

##### **File Permissions:**
```
-rwxr-xr--  alice  staff  document.txt
├─► File type (-=regular file, d=directory)
├─► Owner permissions (rwx)
├─► Group permissions (r-x)  
├─► Other permissions (r--)
├─► Owner name (alice)
└─► Group name (staff)
```

##### **Permission Bits:**
- **Read (r=4)**: View file contents, list directory
- **Write (w=2)**: Modify file contents, create/delete files in directory
- **Execute (x=1)**: Run file as program, access directory

##### **Special Permissions:**
- **SetUID (s)**: Execute with file owner's privileges
- **SetGID (s)**: Execute with file group's privileges  
- **Sticky bit (t)**: Only owner can delete files in directory

#### **Modern Unix ACLs:**
```bash
# setfacl -m user:bob:rw- document.txt
# setfacl -m group:developers:r-- document.txt
# getfacl document.txt
user::rw-
user:bob:rw-
group::r--
group:developers:r--
mask::rw-
other::---
```

#### **Linux Security Modules:**

##### **SELinux (Security-Enhanced Linux):**
- **Type Enforcement**: Subjects and objects have types; policy defines allowed interactions
- **Role-Based Access Control**: Users assigned to roles; roles authorized for domains
- **Multi-Level Security**: Optional confidentiality levels
- **Policy separation**: Policy decisions separated from enforcement

##### **AppArmor:**
- **Path-based**: Controls based on file paths rather than labels
- **Profile-based**: Each application has security profile
- **Learning mode**: Can automatically generate policies

### Windows Access Control

#### **Security Architecture:**

##### **Security Identifiers (SIDs):**
- **Unique identifiers** for users, groups, computers
- **Format**: S-R-I-S-S-S... (Security-Revision-Identifier Authority-Subauthorities)
- **Well-known SIDs**: Built-in accounts (Everyone, Authenticated Users, etc.)

##### **Access Tokens:**
- **User SID**: Primary user identifier
- **Group SIDs**: All groups user belongs to
- **Privileges**: Special rights (backup, debug, etc.)
- **Default DACL**: Default permissions for new objects

##### **Security Descriptors:**
- **Owner**: Who owns the object
- **Primary Group**: Default group for new objects
- **DACL**: Discretionary Access Control List
- **SACL**: System Access Control List (auditing)

#### **Access Control Process:**
```
1. User requests access to object
2. System compares user token to object DACL
3. Access Control Entries processed in order:
   - If DENY ACE matches → Access denied
   - If ALLOW ACE matches → Access granted for those permissions
   - If no ACE matches → Access denied (default deny)
```

#### **Advanced Features:**

##### **Integrity Levels (Vista+):**
- **System**: Critical system processes
- **High**: Administrative processes
- **Medium**: Standard user processes
- **Low**: Internet-facing applications (IE, downloaded files)
- **No Write Up policy**: Lower integrity cannot modify higher integrity

##### **Domains and Trusts:**
- **Domain**: Administrative boundary with shared security database
- **Trust relationships**: Allow cross-domain authentication
- **Forest**: Collection of trusted domains
- **Global Catalog**: Directory service for finding objects

---

## Multilevel Security Models

### Bell-LaPadula Model (Confidentiality)

#### **Core Principles:**
Designed to **prevent unauthorized disclosure** of classified information in military/government systems.

#### **Security Properties:**

##### **Simple Security Property (No Read Up):**
- **Rule**: Subject cannot read objects at higher classification levels
- **Example**: Secret-cleared user cannot read Top Secret documents
- **Purpose**: Prevents unauthorized access to sensitive information

##### **★-Property (No Write Down):**
- **Rule**: Subject cannot write to objects at lower classification levels  
- **Example**: Secret-cleared user cannot write to Unclassified files
- **Purpose**: Prevents information leakage through Trojan horses

#### **Security Levels:**
```
Top Secret    ← Highest sensitivity
     ↑
   Secret
     ↑
Confidential
     ↑
Unclassified  ← Lowest sensitivity
```

#### **Implementation Challenges:**

##### **Trusted Subjects:**
- **Need**: Some subjects must violate ★-property (e.g., declassification)
- **Risk**: Trusted subjects are potential vulnerabilities
- **Control**: Minimize number and scope of trusted subjects

##### **Real-world Workflows:**
- **Collaboration**: Difficult when users at different levels need to work together
- **Practical constraints**: Many legitimate activities require "write down"
- **User resistance**: Rigid policies impede work efficiency

#### **Criticisms:**
- **McLean's theorem**: BLP properties alone insufficient for security
- **Covert channels**: Model doesn't address all information flows
- **Usability**: Too restrictive for most commercial environments

### Biba Model (Integrity)

#### **Core Principles:**
Designed to **prevent unauthorized modification** of data - "Bell-LaPadula upside down"

#### **Security Properties:**

##### **Simple Integrity Property (No Read Down):**
- **Rule**: Subject cannot read objects at lower integrity levels
- **Purpose**: Prevents contamination from unreliable sources
- **Example**: High-integrity process cannot read low-integrity network data

##### **★-Integrity Property (No Write Up):**
- **Rule**: Subject cannot write to objects at higher integrity levels
- **Purpose**: Prevents corruption of trusted data
- **Example**: Low-integrity process cannot modify system files

#### **Integrity Levels:**
```
System     ← Highest integrity (OS, system files)
   ↑
  High     ← Trusted applications
   ↑
 Medium    ← Standard user data
   ↑
  Low      ← Network data, downloads
```

#### **Low Water Mark Principle:**
- **Concept**: Integrity of object is minimum of all contributing objects
- **Example**: Document created from both high and low integrity sources becomes low integrity
- **Implementation**: Track data provenance and adjust integrity levels

#### **Real-world Applications:**

##### **Windows Vista Integrity:**
- **Internet Explorer**: Runs at Low integrity by default
- **Downloaded files**: Marked as Low integrity
- **System protection**: Low integrity processes cannot modify system files
- **User experience**: Minimal impact on normal operations

##### **LOMAC (Linux):**
- **Network isolation**: Processes touching network downgraded to Low
- **Automatic demotion**: No user intervention required
- **System protection**: Compromised network services cannot damage system

### Chinese Wall Model

#### **Core Principles:**
Prevents **conflicts of interest** by ensuring users cannot access information from competing organizations.

#### **Rules:**
1. **Simple Security**: User can access object if no conflict of interest
2. **★-Property**: User can write to object only if:
   - No conflict of interest, OR
   - All objects that can be read are in same conflict class

#### **Example:**
- **Conflict classes**: {Bank A, Bank B}, {Oil Company X, Oil Company Y}
- **User access**: Can access Bank A OR Bank B, but not both
- **Wall effect**: Once user accesses Bank A, "wall" prevents access to Bank B

---

## Covert Channels

### Definition & Characteristics

#### **Covert Channel:**
A **communication path not intended** by system designers that can be used to transfer information in violation of security policy.

#### **Requirements for Covert Channel:**
1. **Shared resource**: Sender and receiver access common resource
2. **Observable variation**: Sender can modify resource property receiver can observe  
3. **Synchronization**: Communication can be coordinated between parties

### Types of Covert Channels

#### **Storage Channels:**
Use **shared storage resources** to signal information
- **File existence**: Create/delete files to signal bits
- **Disk space**: Fill/free disk space to indicate data
- **File attributes**: Modify timestamps, permissions, sizes
- **Process IDs**: Use predictable PID allocation patterns

#### **Timing Channels:**
Use **timing variations** in shared resources to signal information
- **CPU utilization**: High/low usage patterns signal bits
- **Memory access**: Cache hit/miss patterns
- **Network delays**: Induced latency variations
- **System calls**: Response time variations

### Covert Channel Example

#### **File-Based Storage Channel:**
```
Alice (Top Secret clearance) → Bob (Confidential clearance)

Protocol:
- To send '1': Alice creates file "FileXYzW"
- To send '0': Alice ensures file "FileXYzW" doesn't exist
- Every minute: Bob lists directory contents
- Bob reads bit based on file presence

Result: Alice can leak Top Secret information to Bob!
```

#### **Bandwidth Considerations:**
- **100MB Top Secret file**: Would take >25 years at 1 bit/second
- **256-bit AES key**: Only 5 minutes to leak at 1 bit/second
- **Implication**: Even low-bandwidth covert channels can leak sensitive keys

### Covert Channel Mitigation

#### **Elimination Strategies:**
- **Remove shared resources**: Impossible in useful systems
- **Fixed resource allocation**: Give each security level dedicated resources
- **Noise introduction**: Add randomness to resource allocation
- **Scheduling controls**: Deterministic resource scheduling

#### **Reduction Techniques:**
- **DoD guideline**: Reduce capacity to ≤1 bit/second
- **Resource quotas**: Limit resource usage per security level
- **Random delays**: Introduce timing noise
- **Audit and detection**: Monitor for covert channel usage

#### **Practical Approaches:**
- **Accept residual risk**: Perfect elimination impossible
- **Focus on high-bandwidth channels**: Address most dangerous channels first
- **Regular assessment**: Continuous evaluation of new channels
- **Defense in depth**: Multiple mitigation layers

---

## Access Control in Practice

### Implementation Challenges

#### **Complexity Management:**
- **Policy complexity**: Real policies often have thousands of rules
- **Rule conflicts**: Overlapping and contradictory permissions
- **Administration overhead**: Cost of maintaining access control
- **User training**: Ensuring users understand and follow policies

#### **Performance Considerations:**
- **Access checking overhead**: Every access requires permission check
- **Caching strategies**: Balance security with performance
- **Network latency**: Distributed access control decisions
- **Scalability limits**: Performance degradation with system size

#### **Usability vs Security:**
- **User resistance**: Complex policies impede productivity
- **Shadow IT**: Users circumvent controls through unofficial channels
- **Emergency access**: Break-glass procedures for critical situations
- **Social engineering**: Attackers exploit user desire to help

### Break-the-Glass Policies

#### **Concept:**
Allow users to **temporarily override access controls** in emergency situations with appropriate safeguards.

#### **Requirements:**
- **Maximum freedom**: System provides needed access when required
- **Maximum responsibility**: User accepts accountability for override
- **Audit trail**: All override actions logged and monitored
- **Notification**: Supervisors alerted to override usage

#### **Implementation:**
```
1. User attempts normal access → Denied
2. System offers break-glass option with warning
3. User accepts responsibility for override
4. System grants temporary access
5. All actions logged and supervisor notified
6. Post-incident review determines if override was justified
```

### Real-World Applications

#### **Healthcare Systems:**
- **Normal access**: Role-based access to patient records
- **Emergency access**: Break-glass for medical emergencies
- **Privacy protection**: Minimal access principle
- **Audit requirements**: HIPAA compliance monitoring

#### **Financial Services:**
- **Separation of duties**: No single person can complete high-value transactions
- **Dual control**: Critical operations require two authorized users
- **Time-based access**: Trading floor access limited to business hours
- **Regulatory compliance**: SOX, PCI-DSS requirements

#### **Government/Military:**
- **Classification levels**: Multilevel security for sensitive information
- **Need-to-know**: Additional compartmentalization beyond clearance levels
- **Foreign nationals**: Special restrictions on non-citizen access
- **Audit trails**: Detailed logging for security investigations

---

## Common Vulnerabilities & Attacks

### Privilege Escalation

#### **Vertical Escalation:**
- **Definition**: User gains higher-level privileges
- **Examples**: Regular user becomes administrator
- **Attack vectors**: Software vulnerabilities, configuration errors
- **Mitigation**: Principle of least privilege, regular patching

#### **Horizontal Escalation:**
- **Definition**: User gains access to resources at same privilege level
- **Examples**: User A accesses User B's files
- **Attack vectors**: Weak access controls, application flaws
- **Mitigation**: Proper access control implementation

### Confused Deputy Problem

#### **Definition:**
Trusted program **misuses its privileges** on behalf of less-privileged user to access unauthorized resources.

#### **Example:**
```
1. Compiler runs with high privileges to access system files
2. User provides malicious filename pointing to password file  
3. Compiler overwrites password file thinking it's output file
4. User gains unauthorized access to system
```

#### **Mitigation:**
- **Capability-based systems**: Authority follows designation
- **Input validation**: Carefully check all user inputs
- **Principle of least privilege**: Minimize program privileges
- **Sandboxing**: Isolate potentially dangerous operations

### Time-of-Check to Time-of-Use (TOCTTOU)

#### **Definition:**
**Race condition** where resource state changes between access control check and actual use.

#### **Example:**
```
1. Program checks if user can access /tmp/userfile
2. User replaces /tmp/userfile with link to /etc/passwd
3. Program writes to /tmp/userfile (now actually /etc/passwd)
4. System password file corrupted
```

#### **Mitigation:**
- **Atomic operations**: Combine check and use into single operation
- **File locking**: Prevent resource modification during access
- **Unique filenames**: Use unpredictable temporary file names
- **Secure temporary directories**: Proper directory permissions

---

## Key Concepts Summary

### Critical Terms
- **Subject vs Object**: Active entities vs passive resources
- **DAC vs MAC vs RBAC**: Discretionary vs mandatory vs role-based control
- **ACL vs Capabilities**: Column-wise vs row-wise access matrix storage
- **Confidentiality vs Integrity**: Bell-LaPadula vs Biba models
- **Covert vs Overt channels**: Unintended vs intended communication paths

### Design Principles
1. **Principle of least privilege**: Grant minimum necessary permissions
2. **Separation of duties**: Distribute critical functions among multiple people
3. **Defense in depth**: Multiple access control layers
4. **Fail-safe defaults**: Default to deny access
5. **Complete mediation**: Check every access attempt
6. **Open design**: Security through policy, not obscurity

| Model | Primary Goal | Key Properties | Best Use Cases |
|-------|--------------|----------------|----------------|
| **DAC** | Flexibility | Owner control, delegation | Collaborative environments |
| **MAC** | Strong security | System enforcement, labels | Military, government |
| **RBAC** | Administrative ease | Role-based, organizational | Enterprise systems |
| **Bell-LaPadula** | Confidentiality | No read up, no write down | Classified information |
| **Biba** | Integrity | No read down, no write up | System protection |

---

## Practice Questions

### Conceptual Understanding:
1. Explain the fundamental differences between DAC, MAC, and RBAC models.
2. Why are ACLs and capabilities not truly equivalent despite representing the same access matrix?
3. How do covert channels undermine multilevel security models, and why are they impossible to eliminate completely?

### Applied Analysis:
1. Design an access control system for a hospital that handles both normal operations and emergency situations.
2. Compare the security trade-offs between using ACLs versus capabilities for a distributed file system.
3. Analyze how the Bell-LaPadula and Biba models could be combined to protect both confidentiality and integrity.

### Critical Thinking:
1. How might emerging technologies (cloud computing, IoT, mobile devices) challenge traditional access control models?
2. What are the implications of the "capability myths" for modern system design?
3. How do social and organizational factors affect the success of access control implementations?

### Real-World Scenarios:
1. A bank needs to implement access controls for its trading system. What combination of models would you recommend and why?
2. How would you design access controls for a social media platform that needs to scale to billions of users?
3. What access control challenges arise in a "bring your own device" (BYOD) corporate environment?
