# JWT Authentication Setup

## Prerequisites
- MySQL server running on `localhost:3306`
- Default MySQL user: `root` with password `root`

## Database Setup
The database `jwtdb` will be created automatically when the application starts. The `users` table will also be created by Hibernate.

## Running the Application

1. Start MySQL server
2. Run the application:
   ```bash
   mvn spring-boot:run
   ```
   Or build and run the JAR:
   ```bash
   mvn clean package
   java -jar target/jwt-0.0.1-SNAPSHOT.jar
   ```

## API Endpoints

### 1. Register a User
**POST** `/auth/register`

Request body:
```json
{
  "username": "testuser",
  "email": "test@example.com",
  "password": "password123"
}
```

Response:
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### 2. Login
**POST** `/auth/login`

Request body:
```json
{
  "username": "testuser",
  "password": "password123"
}
```

Response:
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### 3. Access Protected Endpoint
**GET** `/api/hello`

Headers:
```
Authorization: Bearer <your_jwt_token>
```

Response:
```json
"Hello from protected endpoint"
```

## Testing with cURL

### Register
```bash
curl -X POST http://localhost:8080/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","email":"test@example.com","password":"password123"}'
```

### Login
```bash
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","password":"password123"}'
```

### Access Protected Resource
```bash
curl -X GET http://localhost:8080/api/hello \
  -H "Authorization: Bearer <your_jwt_token>"
```

## Configuration

Edit `src/main/resources/application.properties` to change:
- Database URL: `spring.datasource.url`
- Database username/password: `spring.datasource.username` and `spring.datasource.password`
- JWT secret: `jwt.secret` (must be Base64 encoded)
- JWT expiration time: `jwt.expiration` (in milliseconds, default: 24 hours)

## Project Structure

- `entity/User.java` - User JPA entity implementing UserDetails
- `repository/UserRepository.java` - User data access layer
- `service/CustomUserDetailsService.java` - Spring Security UserDetailsService implementation
- `security/JwtUtil.java` - JWT token generation and validation
- `security/JwtAuthFilter.java` - JWT authentication filter
- `security/SecurityConfig.java` - Spring Security configuration
- `controller/AuthController.java` - Authentication endpoints
- `controller/TestController.java` - Protected test endpoint
- `dto/` - Data transfer objects (LoginRequest, RegisterRequest, JwtResponse)
