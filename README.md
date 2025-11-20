# 🚀 Spring Boot REST API Demo

![Java](https://img.shields.io/badge/Java-17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.2-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-3.8-C71A36?style=for-the-badge&logo=apache-maven&logoColor=white)

> A comprehensive Spring Boot REST API demonstrating CRUD operations, pagination, sorting, validation, and exception handling with best practices.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [API Endpoints](#-api-endpoints)
- [Code Examples](#-code-examples)
- [Best Practices](#-best-practices)
- [Testing](#-testing)

---

## 🎯 Overview

This project demonstrates a production-ready Spring Boot REST API with comprehensive features including:
- RESTful CRUD operations
- Pagination and sorting
- Request validation
- Custom exception handling
- DTO pattern implementation
- Repository pattern
- Service layer architecture

Perfect for learning Spring Boot API development or as a reference for best practices.

---

## ✨ Features

- ✅ **CRUD Operations** - Create, Read, Update, Delete
- ✅ **Pagination & Sorting** - Handle large datasets efficiently
- ✅ **Request Validation** - Bean Validation (JSR-380)
- ✅ **Exception Handling** - Global exception handler with `@ControllerAdvice`
- ✅ **DTO Pattern** - Clean separation between entities and API responses
- ✅ **Custom Responses** - Standardized API response format
- ✅ **HTTP Status Codes** - Proper REST status codes (200, 201, 204, 404, etc.)
- ✅ **Swagger/OpenAPI** - Interactive API documentation
- ✅ **Unit Tests** - Service and controller layer tests

---

## 🛠️ Tech Stack

- **Java 17** - Modern Java features
- **Spring Boot 3.2** - Application framework
- **Spring Data JPA** - Data persistence
- **PostgreSQL** - Relational database
- **Hibernate** - ORM framework
- **Maven** - Build automation
- **Lombok** - Reduce boilerplate code
- **JUnit 5** - Testing framework
- **Mockito** - Mocking framework
- **Swagger/SpringDoc** - API documentation

---

## 📁 Project Structure

```
spring-boot-rest-api-demo/
├── src/
│   ├── main/
│   │   ├── java/com/demo/api/
│   │   │   ├── controller/
│   │   │   │   └── ProductController.java
│   │   │   ├── dto/
│   │   │   │   ├── ProductDTO.java
│   │   │   │   ├── ProductRequest.java
│   │   │   │   └── ApiResponse.java
│   │   │   ├── entity/
│   │   │   │   └── Product.java
│   │   │   ├── exception/
│   │   │   │   ├── ResourceNotFoundException.java
│   │   │   │   └── GlobalExceptionHandler.java
│   │   │   ├── repository/
│   │   │   │   └── ProductRepository.java
│   │   │   ├── service/
│   │   │   │   ├── ProductService.java
│   │   │   │   └── ProductServiceImpl.java
│   │   │   └── ApiDemoApplication.java
│   │   └── resources/
│   │       ├── application.yml
│   │       └── application-dev.yml
│   └── test/
│       └── java/com/demo/api/
│           ├── controller/
│           │   └── ProductControllerTest.java
│           └── service/
│               └── ProductServiceTest.java
├── pom.xml
└── README.md
```

---

## 🚀 Getting Started

### **Prerequisites**
- Java 17 or higher
- Maven 3.8+
- PostgreSQL 15+

### **Installation**

1. **Clone the repository**
```bash
git clone https://github.com/Solo-Dev27/spring-boot-rest-api-demo.git
cd spring-boot-rest-api-demo
```

2. **Configure Database**
```yaml
# application.yml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/products_db
    username: your_username
    password: your_password
```

3. **Build the project**
```bash
mvn clean install
```

4. **Run the application**
```bash
mvn spring-boot:run
```

5. **Access the API**
- API: http://localhost:8080
- Swagger UI: http://localhost:8080/swagger-ui.html

---

## 📚 API Endpoints

### **Product Management**

| Method | Endpoint | Description | Request Body | Response |
|--------|----------|-------------|--------------|----------|
| GET | `/api/products` | Get all products (paginated) | - | `Page<ProductDTO>` |
| GET | `/api/products/{id}` | Get product by ID | - | `ProductDTO` |
| POST | `/api/products` | Create new product | `ProductRequest` | `ProductDTO` |
| PUT | `/api/products/{id}` | Update product | `ProductRequest` | `ProductDTO` |
| DELETE | `/api/products/{id}` | Delete product | - | `204 No Content` |
| GET | `/api/products/search` | Search products | Query params | `Page<ProductDTO>` |

### **Query Parameters (for GET /api/products)**

- `page` - Page number (default: 0)
- `size` - Page size (default: 20)
- `sort` - Sort field (default: id)
- `direction` - Sort direction (ASC/DESC, default: ASC)
- `search` - Search keyword (optional)

**Example Request:**
```bash
GET /api/products?page=0&size=10&sort=name&direction=ASC&search=laptop
```

---

## 💻 Code Examples

### **1. Entity**
```java
@Entity
@Table(name = "products")
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Product {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(length = 500)
    private String description;
    
    @Column(nullable = false)
    private BigDecimal price;
    
    @Column(nullable = false)
    private Integer quantity;
    
    @Column(name = "created_at", nullable = false, updatable = false)
    private LocalDateTime createdAt;
    
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;
    
    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
    }
    
    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }
}
```

### **2. DTO (Data Transfer Object)**
```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class ProductDTO {
    private Long id;
    private String name;
    private String description;
    private BigDecimal price;
    private Integer quantity;
    private LocalDateTime createdAt;
    private LocalDateTime updatedAt;
}

@Data
@NoArgsConstructor
@AllArgsConstructor
public class ProductRequest {
    
    @NotBlank(message = "Product name is required")
    @Size(min = 3, max = 100, message = "Name must be between 3 and 100 characters")
    private String name;
    
    @Size(max = 500, message = "Description cannot exceed 500 characters")
    private String description;
    
    @NotNull(message = "Price is required")
    @DecimalMin(value = "0.0", inclusive = false, message = "Price must be greater than 0")
    private BigDecimal price;
    
    @NotNull(message = "Quantity is required")
    @Min(value = 0, message = "Quantity cannot be negative")
    private Integer quantity;
}
```

### **3. Repository**
```java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
    
    Page<Product> findByNameContainingIgnoreCase(String name, Pageable pageable);
    
    Optional<Product> findByName(String name);
    
    @Query("SELECT p FROM Product p WHERE p.price BETWEEN :minPrice AND :maxPrice")
    List<Product> findByPriceRange(@Param("minPrice") BigDecimal minPrice, 
                                   @Param("maxPrice") BigDecimal maxPrice);
}
```

### **4. Service Layer**
```java
public interface ProductService {
    Page<ProductDTO> getAllProducts(Pageable pageable);
    ProductDTO getProductById(Long id);
    ProductDTO createProduct(ProductRequest request);
    ProductDTO updateProduct(Long id, ProductRequest request);
    void deleteProduct(Long id);
    Page<ProductDTO> searchProducts(String keyword, Pageable pageable);
}

@Service
@RequiredArgsConstructor
public class ProductServiceImpl implements ProductService {
    
    private final ProductRepository productRepository;
    
    @Override
    public Page<ProductDTO> getAllProducts(Pageable pageable) {
        return productRepository.findAll(pageable)
            .map(this::convertToDTO);
    }
    
    @Override
    public ProductDTO getProductById(Long id) {
        Product product = productRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("Product", "id", id));
        return convertToDTO(product);
    }
    
    @Override
    @Transactional
    public ProductDTO createProduct(ProductRequest request) {
        Product product = new Product();
        product.setName(request.getName());
        product.setDescription(request.getDescription());
        product.setPrice(request.getPrice());
        product.setQuantity(request.getQuantity());
        
        Product saved = productRepository.save(product);
        return convertToDTO(saved);
    }
    
    @Override
    @Transactional
    public ProductDTO updateProduct(Long id, ProductRequest request) {
        Product product = productRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("Product", "id", id));
        
        product.setName(request.getName());
        product.setDescription(request.getDescription());
        product.setPrice(request.getPrice());
        product.setQuantity(request.getQuantity());
        
        Product updated = productRepository.save(product);
        return convertToDTO(updated);
    }
    
    @Override
    @Transactional
    public void deleteProduct(Long id) {
        Product product = productRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("Product", "id", id));
        productRepository.delete(product);
    }
    
    private ProductDTO convertToDTO(Product product) {
        return new ProductDTO(
            product.getId(),
            product.getName(),
            product.getDescription(),
            product.getPrice(),
            product.getQuantity(),
            product.getCreatedAt(),
            product.getUpdatedAt()
        );
    }
}
```

### **5. Controller**
```java
@RestController
@RequestMapping("/api/products")
@RequiredArgsConstructor
public class ProductController {
    
    private final ProductService productService;
    
    @GetMapping
    public ResponseEntity<Page<ProductDTO>> getAllProducts(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "20") int size,
        @RequestParam(defaultValue = "id") String sort,
        @RequestParam(defaultValue = "ASC") String direction
    ) {
        Sort.Direction sortDirection = Sort.Direction.fromString(direction);
        Pageable pageable = PageRequest.of(page, size, Sort.by(sortDirection, sort));
        Page<ProductDTO> products = productService.getAllProducts(pageable);
        return ResponseEntity.ok(products);
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<ProductDTO> getProductById(@PathVariable Long id) {
        ProductDTO product = productService.getProductById(id);
        return ResponseEntity.ok(product);
    }
    
    @PostMapping
    public ResponseEntity<ProductDTO> createProduct(@Valid @RequestBody ProductRequest request) {
        ProductDTO created = productService.createProduct(request);
        return ResponseEntity.status(HttpStatus.CREATED).body(created);
    }
    
    @PutMapping("/{id}")
    public ResponseEntity<ProductDTO> updateProduct(
        @PathVariable Long id,
        @Valid @RequestBody ProductRequest request
    ) {
        ProductDTO updated = productService.updateProduct(id, request);
        return ResponseEntity.ok(updated);
    }
    
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteProduct(@PathVariable Long id) {
        productService.deleteProduct(id);
        return ResponseEntity.noContent().build();
    }
    
    @GetMapping("/search")
    public ResponseEntity<Page<ProductDTO>> searchProducts(
        @RequestParam String keyword,
        @PageableDefault(size = 20) Pageable pageable
    ) {
        Page<ProductDTO> products = productService.searchProducts(keyword, pageable);
        return ResponseEntity.ok(products);
    }
}
```

### **6. Exception Handling**
```java
@Data
@AllArgsConstructor
public class ErrorResponse {
    private int status;
    private String message;
    private String path;
    private LocalDateTime timestamp;
}

public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String resource, String field, Object value) {
        super(String.format("%s not found with %s: '%s'", resource, field, value));
    }
}

@ControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(
        ResourceNotFoundException ex, 
        WebRequest request
    ) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.NOT_FOUND.value(),
            ex.getMessage(),
            request.getDescription(false).replace("uri=", ""),
            LocalDateTime.now()
        );
        return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
    }
    
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, Object>> handleValidationErrors(
        MethodArgumentNotValidException ex
    ) {
        List<String> errors = ex.getBindingResult()
            .getFieldErrors()
            .stream()
            .map(error -> error.getField() + ": " + error.getDefaultMessage())
            .collect(Collectors.toList());
        
        Map<String, Object> errorResponse = new HashMap<>();
        errorResponse.put("status", HttpStatus.BAD_REQUEST.value());
        errorResponse.put("message", "Validation failed");
        errorResponse.put("errors", errors);
        errorResponse.put("timestamp", LocalDateTime.now());
        
        return new ResponseEntity<>(errorResponse, HttpStatus.BAD_REQUEST);
    }
    
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGlobalException(
        Exception ex, 
        WebRequest request
    ) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.INTERNAL_SERVER_ERROR.value(),
            "An unexpected error occurred",
            request.getDescription(false).replace("uri=", ""),
            LocalDateTime.now()
        );
        return new ResponseEntity<>(error, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

---

## 🎯 Best Practices Demonstrated

1. ✅ **Layered Architecture** - Controller → Service → Repository
2. ✅ **DTO Pattern** - Separate API contracts from entities
3. ✅ **Input Validation** - Bean Validation annotations
4. ✅ **Exception Handling** - Centralized with `@ControllerAdvice`
5. ✅ **HTTP Status Codes** - Proper REST semantics (200, 201, 204, 404, 400, 500)
6. ✅ **Pagination** - Handle large datasets efficiently
7. ✅ **Sorting** - Flexible sorting options
8. ✅ **Search/Filter** - Query capabilities
9. ✅ **Transaction Management** - `@Transactional` for data consistency
10. ✅ **Clean Code** - Meaningful names, single responsibility

---

## ✅ Testing

### **Unit Tests**

```java
@ExtendWith(MockitoExtension.class)
class ProductServiceTest {
    
    @Mock
    private ProductRepository productRepository;
    
    @InjectMocks
    private ProductServiceImpl productService;
    
    @Test
    void shouldCreateProduct() {
        // Given
        ProductRequest request = new ProductRequest("Laptop", "Gaming laptop", 
                                                    new BigDecimal("1500.00"), 10);
        Product product = new Product();
        product.setId(1L);
        product.setName(request.getName());
        
        when(productRepository.save(any(Product.class))).thenReturn(product);
        
        // When
        ProductDTO result = productService.createProduct(request);
        
        // Then
        assertNotNull(result);
        assertEquals("Laptop", result.getName());
        verify(productRepository, times(1)).save(any(Product.class));
    }
    
    @Test
    void shouldThrowExceptionWhenProductNotFound() {
        // Given
        Long productId = 99L;
        when(productRepository.findById(productId)).thenReturn(Optional.empty());
        
        // When & Then
        assertThrows(ResourceNotFoundException.class, () -> {
            productService.getProductById(productId);
        });
    }
}
```

### **Run Tests**

```bash
# Run all tests
mvn test

# Run with coverage
mvn clean test jacoco:report

# View coverage report
open target/site/jacoco/index.html
```

---

## 📄 License

This project is licensed under the MIT License.

---

## 👤 Author

**Solomon Ilegar**
- GitHub: [@Solo-Dev27](https://github.com/Solo-Dev27)
- LinkedIn: [Solomon Ilegar](https://www.linkedin.com/in/solomonilegar/)

---

**⭐ If you find this project helpful, please give it a star!**
