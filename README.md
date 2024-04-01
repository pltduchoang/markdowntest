Comprehensive Security Assessment
=================================

### Use of AsyncStorage

**Type**: Insecure Data Storage\
**Explanation**: AsyncStorage is an unencrypted key-value storage system. Storing sensitive information such as usernames, passwords, and personal notes without encryption can expose this data to anyone with access to the file system, leading to data leakage.

### Hardcoded User Credentials

**Type**: Improper Authentication\
**Explanation**: The app uses a simple array to check usernames and passwords, which is a static and insecure method. Real-world applications require dynamic validation against a secure database with hashed passwords. Additionally, storing passwords in plaintext within the application code or in AsyncStorage is highly insecure.

### Display Password Input

**Type**: Improper Authentication\
**Explanation**: Displaying passwords as plain text instead of masking them exposes user credentials to shoulder-surfing attacks, where an attacker can simply look over the user's shoulder to steal their password.

### Use of Eval Method

**Type**: Code Injection\
**Explanation**: `eval` executes the string passed to it as JavaScript code, which can lead to arbitrary code execution if an attacker manages to inject malicious code into the note text. This is a critical vulnerability that can lead to a wide range of attacks including stealing cookies, executing unauthorized actions on behalf of the user, or loading external scripts.

### Lack of Input Validation and Sanitization

**Type**: Insufficient Input Validation\
**Explanation**: The application does not validate or sanitize the input for note titles and equations, leading to potential stored cross-site scripting (XSS) if the application were to be adapted for web, or code injection vulnerabilities through the use of `eval` in a mobile context. Malicious input can be crafted to exploit these vulnerabilities.

### Inclusion of User Credentials in Storage Keys and Insecure Handling of Authentication States

**Type**: Insecure Code Practices\
**Explanation**: Concatenating usernames and passwords for use in storage keys or any part of the code is insecure. It exposes sensitive information unnecessarily and increases the risk of credential leakage.

* * * * *

Improvement Work Report
=================================

### Secure Storage of Sensitive Data

**Implementation**: The updated app uses AsyncStorage to store JWT tokens, which are critical for maintaining user sessions. While AsyncStorage is a key-value storage system and not inherently encrypted, it's a step forward in managing session tokens. This helps avoid storing username and password in plain text in the local storage. In real-world applications, JWT also has a time validation, which is a highly recommended practice to control authentication sessions.\
**Reflection**: For enhanced security, consider using a more secure storage solution like react-native-secure-storage, which encrypts the data before storing it. Storing sensitive information like tokens in an encrypted format is crucial to prevent unauthorized access, especially if the device is compromised.

### Secure Authentication Practices

**Implementation**: The app has transitioned to using JWT for authentication, eliminating hardcoded credentials and relying on a backend service for user verification. The login process securely communicates with the backend and stores the returned JWT for session management.\
**Reflection**: Secure authentication practices, including the use of JWTs, help in maintaining user sessions securely and providing a way to communicate securely with backend services. Using tokens instead of credentials for subsequent requests minimizes the risk of credential interception and misuse.

### Input Validation and Sanitization

**Implementation**: The Notes component validates the format of equations before adding a new note. This approach helps prevent potentially malicious or malformed input from being processed.\
**Reflection**: Proper input validation and sanitization are crucial for preventing injection attacks, including code injection and SQL injection, depending on the context. Validating input on the client side improves user experience by providing immediate feedback, while server-side validation is critical for security.

### Rectifying Insecure Code Practices

**Implementation**: The updated code removes the use of `eval` for evaluating mathematical expressions, replacing it with a safer, custom evaluation function. It also removes hardcoded credentials, using a more secure authentication mechanism with JWT.\
**Reflection**: Insecure code practices, such as using `eval` or including hardcoded credentials, can lead to a variety of security vulnerabilities. Removing these practices reduces the surface area for potential attacks, making the app more secure.

### Additional Reflections

**Importance of Security Measures**: Implementing these security measures is crucial for protecting user data, ensuring the integrity of the application, and maintaining trust. Insecure applications can lead to data breaches, unauthorized access, and other security incidents that can have severe consequences for both users and developers.

**Lessons Learned:**

-   Secure Authentication: Using modern authentication mechanisms like JWT and ensuring credentials are not stored or transmitted insecurely.
-   Data Storage: The importance of encrypting sensitive data, even on the client side, to protect against data theft.
-   Input Validation: The necessity of validating and sanitizing inputs to prevent injection attacks.
-   Avoiding Insecure Practices: The need to avoid insecure coding practices like using `eval` or storing sensitive information in plaintext.
