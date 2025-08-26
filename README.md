# Debug Task – Spring Boot Login API

## Objective

You are provided with a basic Spring Boot application containing a login endpoint at `/api/auth/login`. The endpoint is intended to authenticate users based on email and password. However, all login attempts currently result in a `401 Unauthorized` response, even when valid credentials are used.

Your task is to:

- Identify and resolve the issue(s) preventing successful authentication.
- Ensure that valid login attempts return a `200 OK` response along with a dummy JWT token.
- Ensure invalid login attempts return a proper `401 Unauthorized` response.
- Optionally improve the overall code quality, structure, or error handling where appropriate.

## Task Instructions

1. Clone or extract the provided project and open it in your preferred IDE.
2. Run the application using the standard Spring Boot method.
3. Attempt a login request using tools such as Postman or curl.
4. Diagnose why the authentication flow is not working as expected.
5. Fix the issues and test for the following:
   - Valid credentials return HTTP 200 with a sample JWT token.
   - Invalid credentials return HTTP 401 Unauthorized.
6. Keep the JWT logic simple—return a static token or placeholder string.

## Sample Credentials

Use the following sample user credentials (you may seed this user manually in memory or via data.sql):

Email: test@example.com  
Password: password123

Note: Ensure that the password is stored in a way that aligns with how Spring Security expects to verify it (e.g., password encoding if applicable).

## Evaluation Criteria

- Ability to identify and resolve issues related to authentication and configuration.
- Understanding of Spring Security concepts and correct usage.
- Clarity, readability, and maintainability of code changes.
- Appropriate use of HTTP status codes and request/response patterns.
- Optional: Suggestions or implementation of code improvements.

## Submission Instructions

- Submit the modified project as a ZIP file or via a public Git repository link.
- Include a brief section in this README file under **"Fix Summary"** explaining:
  - What issues were identified
  - What changes were made to fix them
  - Any improvements or recommendations made beyond the fix

## Fix Summary (to be filled by the candidate)

- Identified Issues:
  - H2 database treats USER as a reserved keyword, causing table creation and query errors.
  - Hibernate/JPA generated SQL did not quote the USER table name, leading to syntax errors.
  - Spring Security was not using the custom UserDetailsService and PasswordEncoder due to missing authentication provider registration.
  - The login endpoint did not handle authentication failures with proper HTTP 401 responses.
  - The password in the database was stored as a BCrypt hash, but configuration or entity mapping issues prevented successful authentication.
  - The USER table was not always created before data.sql ran, causing data insertion failures.
  - No validation on login request payload.

- Fixes Applied:
  - Added @Table(name = "\"USER\"") to the User entity to force Hibernate to use quoted table names.
  - Created schema.sql to explicitly create the USER table before data.sql runs.
  - Set spring.jpa.hibernate.ddl-auto=create in application.properties to ensure schema is created on startup.
  - Registered DaoAuthenticationProvider with the SecurityFilterChain to ensure Spring Security uses the custom UserDetailsService and PasswordEncoder.
  - Updated AuthController to handle AuthenticationException and return HTTP 401 for invalid credentials.
  - Used @Valid for login request validation.
  - Ensured the login endpoint returns a dummy JWT token for successful authentication.

- Improvements or Suggestions (optional):
  - Consider using a dedicated DTO for login responses to improve clarity and extensibility.
  - Add logging for authentication failures and successes for better traceability.
  - Use environment-specific configuration for database and security settings in production.
  - Implement real JWT token generation and validation for production use.
