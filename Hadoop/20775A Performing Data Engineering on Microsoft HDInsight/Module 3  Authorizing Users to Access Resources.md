# Module 3 : Authorizing Users to Access Resources
### Module Overview

- <u>Non-domain</u> joined HDInsight clusters
- Configuring domain-joined HDInsight clusters
- Manage domain-joined HDInsight clusters
- 

## Lesson 1: Non-domain joined HDInsight clusters
- Introduction to non-domain joined HDInsight clusters
- Authentication using an Ambari Admin user
- Authentication using an SSH user
- Demonstration: Authorizing user access to cluster objects



### Introduction to non-domain joined HDInsight clusters
Non-domain joined HDInsight clusters:

- Single user local admin account
- <u>Cannot connect to Azure Active Directory (Azure AD) through Azure Active Directory Domain Services (Azure AD DS)</u>

Good for:

- Application teams or small departments (크지않은 조직에서 쓰는것이 좋다)

Not good for:

- Large enterprises



### Authentication using an Ambari Admin user

- Important to set up Ambari Admin or HTTP user

- Use these credentials to access Ambari and other services 

  - NameNode

  - Oozie

- Ambari Admin can:

  - Create and manage alerts

  - Monitor cluster health 

- Ambari Admin cannot:

  - Manage users, permissions or groups unless cluster is domain-joined

  

### Authentication using an SSH user

- Authentication using SSH user

  - Secure Shell (SSH)
  - Connecting cluster using SSH Password
  - Connecting cluster using SSH Keys
  - SSH Tunneling

  

### Demonstration: Authorizing user access to cluster objects
In this demonstration, you will see how to:

- Access cluster resources using the Ambari dashboard
- Access cluster resources using an SSH tunnel
- Use an SSH tunnel to access the Name node UI



## Lesson 2: Configuring domain-joined HDInsight clusters
- Domain-joined HDInsight clusters
- Azure AD Domain Services
- Introducing Apache Ranger
- Creating a domain-joined HDInsight cluster



### Domain-joined HDInsight clusters

- Domain-joined HDInsight clusters:
  - Support for multiple admin accounts
  - Connects to Azure AD through:
    - Azure AD DS
    - Apache Ranger
- Enables:

  - Network level security

  - Authentication through Azure AD

  - Multiuser authentication

  - Role-based access control (RBAC)
- Good for large enterprises
- Less suitable for application teams or small departments



### Azure AD Domain Services

- Azure AD DS provides:
  - Domain join
  - Group policy
  - Lightweight Directory Access Protocol (LDAP)
  - Kerberos
  - NT LAN Manager (NTLM) authentication

- Azure AD DS is compatible with Server AD

- Use Azure AD DS to authenticate to:
  - PowerShell endpoints
  - Ambari Views
  - ODBC (Open DataBase Connectivity)
  - REST APIs (Representational State Transfer)

> **ODBC**(Open DataBase Connectivity)는 [마이크로소프트](https://ko.wikipedia.org/wiki/%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%86%8C%ED%94%84%ED%8A%B8)가 만든, [데이터베이스](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4)에 접근하기 위한 소프트웨어의 표준 규격으로, 각 데이터베이스의 차이는 ODBC 드라이버에 흡수되기 때문에 사용자는 ODBC에 정해진 순서에 따라서 프로그램을 쓰면 접속처의 데이터베이스가 어떠한 데이터베이스 관리 시스템에 관리되고 있는지 의식할 필요 없이 접근할 수 있다.

> **REST**는 Representational State Transfer라는 용어의 약자로서 2000년도에 로이 필딩 (Roy Fielding)의 박사학위 논문에서 최초로 소개되었습니다. 로이 필딩은 HTTP의 주요 저자 중 한 사람으로 그 당시 웹(HTTP) 설계의 우수성에 비해 제대로 사용되어지지 못하는 모습에 안타까워하며 웹의 장점을 최대한 활용할 수 있는 아키텍처로써 REST를 발표했다고 합니다.



### Introducing Apache Ranger

- Introducing Apache Ranger

  - Apache Ranger
  - Encryption

  

### Creating a domain-joined HDInsight cluster
1. Enable the Azure AD DS for your Azure AD tenant
   - Create the Azure AD DC administrators’ group
   - Create an Azure virtual network
   - Enable Azure AD DS
   - Update the DNS settings for the Azure virtual network
   - Enable password synchronization to AD DS
2. Create a Resource Manager virtual network for the HDInsight cluster
3. Peer the Azure and HDInsight virtual networks
4. Create an HDInsight cluster with domain configuration



### Lesson 3: Manage domain-joined HDInsight clusters
- Introducing Ranger Admin
- Introducing the cluster admin domain user
- Introducing Regular users
- Managing clusters with Ambari UI
- Configure permissions
- Configure Hive policies



### Introducing Ranger Admin

- Ranger Admin:

  - Local Apache Ranger admin account

  - Used to set up policies and manage user access controls

  - Can make other users admins or delegated admins
- Default username: **admin**
- Default password: same as the Ambari admin password



### Introducing the cluster admin domain user
Cluster admin domain user account:

- Azure AD domain user
- Used as the admin for Ambari and Ranger
- Has the following privileges:
  - Join cluster nodes to the domain and place nodes within a particular OU
  - Create service principals within the OU
  - Create reverse Domain Name System (DNS) entries

  

### Introducing Regular users

Regular users:

- Domain users
- Can only access Ranger-managed endpoints, such as HiveServer2
- Subject to Ranger-managed RBAC policies and auditing
- Use Azure AD groups to sync users to Ranger and Ambari



### Managing clusters with Ambari UI

- Roles used for managing clusters with Ambari UI:

  - Cluster administrator

  - Cluster operator

  - Service administrator

  - Service operator

  - Cluster user

     

### Configure permissions

Use Ambari UI to:

- Assign permissions to Hive Views
- Specify users and groups that use these views
- Grant permissions to cluster management roles



### Configure Hive policies

- Domain-joined HDInsight clusters use Hive policies
- You use Hive policies to control access to tables and columns in a Hive database
- Cluster administrator manages and audits access to Hive objects
- Connect to Hive objects through endpoints including:
  - ODBC
  - Ambari Views

  

### Lab: Authorize users to access resources
- Exercise 1: Prepare the lab environment
- Exercise 2: Manage a non-domain joined cluster