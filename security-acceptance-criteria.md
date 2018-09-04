# Security Acceptance Criteria

More often, existing user stories will contain security requirements as acceptance criteria. Examples follow below.

## Application Architecture and Design

- Data flow and process flow diagrams are current and assessed for risk by Information Security
- System and application components are hardened according to Information Security requirements and checked for vulnerabilities through manual or automated security testing. All critical and high vulnerabilities will be remediated prior to release.
- Components are segregated based on compliance requirements and Information Security Data Handling Requirements.

## Authentication, Authorization, Accounting

- All pages and resources require authentication except those specifically intended to be public.
- Forms containing credentials are not filled in by the application. Pre-populating a form could mean that credentials are stored in plaintext or a reversible format.
- All authentication controls are enforced on the server side.
- All authentication controls fail securely to ensure attackers cannot log in.
- All authentication decisions can be logged, without storing sensitive session identifiers or passwords. This should include requests with relevant metadata needed for security investigations.
- Information enumeration is not possible via login, password reset, or forgot account functionality.
- Verify there are no default passwords in use for the application framework or any components used by the application (such as "admin/password").
- Anti-automation is in place to prevent breached credential testing, brute forcing, and account lockout attacks.
- All authentication credentials for accessing services external to the application are encrypted and stored in a protected location.
- Account lockout is divided into soft and hard lock status, and these are not mutually exclusive. If an account is temporarily soft locked out due to a brute force attack, this should not reset the hard lock status.
- If shared knowledge based questions (also known as "secret questions") are required, the questions must not violate privacy laws and are sufficiently strong to protect accounts from malicious recovery.
- The system can be configured to disallow the use of a configurable number of previous passwords.
- All authentication challenges, whether successful or failed, should respond in the same average response time.
- Secrets, API keys, and passwords must not be included in the source code, or online source code repositories.
- Administrative interfaces are not accessible to untrusted parties.

## Access Control

- Directory browsing is disabled unless deliberately desired. Additionally, applications should not allow discovery or disclosure of file or directory metadata, such as Thumbs.db, .DS_Store, .git or .svn folders.
- Verify that all user and data attributes and policy information used by access controls cannot be manipulated by end users unless specifically authorized.
- A centralized mechanism (including libraries that call external authorization services) is used for protecting access to each type of protected resource.
- All access control decisions can be logged and all failed decisions are logged.
- The application has additional authorization (such as step up or adaptive authentication) for lower value systems, and / or segregation of duties for high value applications to enforce anti-fraud controls as per the risk of application and past fraud.
- The application correctly enforces context-sensitive authorization to not allow unauthorized manipulation by means of parameter tampering.

## Input Validation Handling

- The runtime environment is not susceptible to buffer overflows, or that security controls prevent buffer overflows.
- Input validation routines are enforced on the server side and input validation failures result in request rejection and are logged.
- A single input validation control is used by the application for each type of data that is accepted.
- All SQL queries, HQL, OSQL, NOSQL and stored procedures, calling of stored procedures are protected using prepared statements or query parameterization, and thus not susceptible to SQL injection
- The application is not susceptible to LDAP Injection, or that security controls prevent LDAP Injection.
- The application is not susceptible to OS Command Injection, or that security controls prevent OS Command Injection.
- The application is not susceptible to Remote File Inclusion (RFI) or Local File Inclusion (LFI) when content is used that is a path to a file.
- The application is not susceptible to common XML attacks, such as XPath query tampering, XML External Entity attacks, and XML injection attacks.
- Ensure that all string variables placed into HTML or other web client code is either properly contextually encoded manually, or utilize templates that automatically encode contextually to ensure the application is not susceptible to reflected, stored and DOM Cross-Site Scripting (XSS) attacks.
- If the application framework allows automatic mass parameter assignment (also called automatic variable binding) from the inbound request to a model, verify that security sensitive fields such as "accountBalance", "role" or "password" are protected from malicious automatic binding.
- Verify that the application has defenses against HTTP parameter pollution attacks, particularly if the application framework makes no distinction about the source of request parameters (GET, POST, cookies, headers, environment, etc.)
- All input data is validated, not only HTML form fields but all sources of input such as REST calls, query parameters, HTTP headers, cookies, batch files, RSS feeds, etc; using positive validation (whitelisting), then lesser forms of validation such as greylisting (eliminating known bad strings), or rejecting bad inputs (blacklisting).
- Structured data is strongly typed and validated against a defined schema including allowed characters, length and pattern (e.g. credit card numbers or telephone, or validating that two related fields are reasonable, such as validating suburbs and zip or post codes match).
- Unstructured data is sanitized to enforce generic safety measures such as allowed characters and length, and characters potentially harmful in given context should be escaped (e.g. natural names with Unicode or apostrophes, such as ねこ or O'Hara)
- Untrusted HTML from WYSIWYG editors or similar are properly sanitized with an HTML sanitizer and handle it appropriately according to the input validation task and encoding task.
- For auto-escaping template technology, if UI escaping is disabled, ensure that HTML sanitization is enabled instead.
- Data transferred from one DOM context to another, uses safe JavaScript methods, such as using .innerText and .val.
- When parsing JSON in browsers, that JSON.parse is used to parse JSON on the client. Do not use eval() to parse JSON on the client.
- Authenticated data is cleared from client storage, such as the browser DOM, after the session is terminated.

## Encryption At Rest

- All cryptographic modules fail securely, and errors are handled in a way that does not enable oracle padding.
- Cryptographic algorithms used by the application have been validated against FIPS 140-2 or an equivalent standard.
- Sensitive passwords or key material maintained in memory is overwritten with zeros as soon as it no longer required, to mitigate memory dumping attacks.
- Verify that all keys and passwords are replaceable, and are generated or replaced at installation time.
- Random numbers are created with proper entropy even when the application is under heavy load, or that the application degrades gracefully in such circumstances.

## Error Handling and Logging

- Security logs are protected from unauthorized access and modification.
- All non-printable symbols and field separators are properly encoded in log entries, to prevent log injection.
- Log fields from trusted and untrusted sources are distinguishable in log entries and audit log or similar allows for non-repudiation of key transactions.
- The logs are stored on a different partition than the application is running with proper log rotation.
- Time sources should be synchronized to ensure logs have the correct time.

## Data Protection and Verification

- List classification of data processed by the application according to Software Company’s Information Classification Standard. This should be documented in data flow diagrams. There must be explicit enforcement in accordance with the Software company Data Handling Matrix requirements.
- Verify all sensitive data is sent to the server in the HTTP message body or headers (i.e., URL parameters are never used to send sensitive data).
- The application sets appropriate anti-caching headers as per the risk tier of the application and data it handles.
- On the server, all cached or temporary copies of sensitive data stored are protected from unauthorized access or purged/invalidated after the authorized user accesses the sensitive data.
- Verify the application minimizes the number of parameters in a request, such as hidden fields, Ajax variables, cookies and header values.
- Data stored in client side storage (such as HTML5 local storage, session storage, IndexedDB, regular cookies or Flash cookies) does not contain sensitive data or PII.
- Sensitive information maintained in memory is overwritten with zeros as soon as it no longer required, to mitigate memory dumping attacks.

## Communications Security

- Verify that a path can be built from a trusted CA to each Transport Layer Security (TLS) server certificate, and that each server certificate is valid. Also ensure that certificate paths are built and verified for all client certificates using configured trust anchors and revocation information.
- Approved version of TLS and set of cipher suites is used for connections (including both external and backend connections) that pass data classified as RESTRICTED or CONFIDENTIAL according to the Software company Information Classification Standard on public networks, and that it does not fall back to insecure or unencrypted protocols. Ensure the strongest alternative is the preferred algorithm.
- Ensure that a single standard TLS implementation is used by the application and components, that it is configured in accordance with Information Security’s Encryption Standard.

## HTTP Security Configuration Verification

- Application accepts only a defined set of required HTTP request methods, such as GET and POST are accepted, and unused methods (e.g. TRACE, PUT, and DELETE) are explicitly blocked.
- Every HTTP response contains a content type header specifying a safe character set (e.g., UTF-8, ISO 8859-1).
- HTTP headers added by a trusted proxy or SSO devices, such as a bearer token, are authenticated by the application.
- Suitable X-FRAME-OPTIONS headers are in use for sites where content should not be viewed in a 3rd-party X-Frame.
- HTTP headers or any part of the HTTP response do not expose detailed version information of system components.
- Verify that all API responses contain X-Content- Type-Options: nosniff and Content-Disposition: attachment; filename="api.json" (or other appropriate filename for the content type).
- Content security policy (CSPv2) is in place that helps mitigate common DOM, XSS, JSON, and JavaScript injection vulnerabilities.
- X-XSS-Protection: 1; mode=block header is in place to enable browser reflected XSS filters.

## Files and Resources Verification

- Avoid Flash, Active-X, Silverlight, NACL, client- side Java or other client side technologies not supported natively via W3C browser standards.
- Enforce Same-Origin Policy and/or implement Content Security Policy to protect against XSS, Clickjacking and other code injection attacks.