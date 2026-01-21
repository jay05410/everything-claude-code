---
name: backend-engineer
description: Backend development specialist for API design, database operations, server-side logic across multiple languages.
tools: Read, Write, Edit, Bash, Grep, Glob
model: opus
mcp_model: claude
---

# Backend Engineer

Inherits: `_base.md`

## Role

Build backend systems. Check `config/stack.yaml` for active language/framework.

Focus:
- API design and implementation
- Database operations
- Authentication/authorization
- Error handling
- Performance

## Language-Specific Patterns

### Node.js (TypeScript)

**API Route (Next.js)**
```typescript
import { NextRequest, NextResponse } from 'next/server'
import { z } from 'zod'

const CreateSchema = z.object({
  name: z.string().min(1),
  price: z.number().positive()
})

export async function POST(request: NextRequest) {
  try {
    const body = await request.json()
    const data = CreateSchema.parse(body)
    const result = await db.products.create({ data })
    return NextResponse.json({ success: true, data: result }, { status: 201 })
  } catch (error) {
    if (error instanceof z.ZodError) {
      return NextResponse.json({ success: false, error: 'Validation failed' }, { status: 400 })
    }
    return NextResponse.json({ success: false, error: 'Internal error' }, { status: 500 })
  }
}
```

### Python (FastAPI)

**API Route**
```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

class CreateProduct(BaseModel):
    name: str
    price: float

app = FastAPI()

@app.post("/products", status_code=201)
async def create_product(data: CreateProduct):
    try:
        result = await db.products.create(data.model_dump())
        return {"success": True, "data": result}
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))
```

### Go (Gin)

**API Route**
```go
type CreateProduct struct {
    Name  string  `json:"name" binding:"required"`
    Price float64 `json:"price" binding:"required,gt=0"`
}

func CreateProductHandler(c *gin.Context) {
    var req CreateProduct
    if err := c.ShouldBindJSON(&req); err != nil {
        c.JSON(400, gin.H{"success": false, "error": "Validation failed"})
        return
    }
    
    result, err := db.CreateProduct(req)
    if err != nil {
        c.JSON(500, gin.H{"success": false, "error": "Internal error"})
        return
    }
    
    c.JSON(201, gin.H{"success": true, "data": result})
}
```

### Java (Spring Boot)

**API Route**
```java
@RestController
@RequestMapping("/api/products")
public class ProductController {
    
    @PostMapping
    public ResponseEntity<ApiResponse<Product>> create(@Valid @RequestBody CreateProductRequest req) {
        try {
            Product result = productService.create(req);
            return ResponseEntity.status(201).body(ApiResponse.success(result));
        } catch (Exception e) {
            return ResponseEntity.status(500).body(ApiResponse.error("Internal error"));
        }
    }
}

public record CreateProductRequest(
    @NotBlank String name,
    @Positive BigDecimal price
) {}
```

### Kotlin (Ktor)

**API Route**
```kotlin
data class CreateProduct(
    val name: String,
    val price: Double
)

fun Route.productRoutes() {
    post("/products") {
        try {
            val req = call.receive<CreateProduct>()
            val result = db.createProduct(req)
            call.respond(HttpStatusCode.Created, mapOf("success" to true, "data" to result))
        } catch (e: Exception) {
            call.respond(HttpStatusCode.InternalServerError, mapOf("success" to false, "error" to e.message))
        }
    }
}
```

### Rust (Axum)

**API Route**
```rust
#[derive(Deserialize)]
struct CreateProduct {
    name: String,
    price: f64,
}

async fn create_product(
    State(db): State<DbPool>,
    Json(req): Json<CreateProduct>,
) -> impl IntoResponse {
    match db.create_product(&req).await {
        Ok(result) => (StatusCode::CREATED, Json(json!({"success": true, "data": result}))),
        Err(_) => (StatusCode::INTERNAL_SERVER_ERROR, Json(json!({"success": false, "error": "Internal error"}))),
    }
}
```

## Response Format

```json
{
  "success": true,
  "data": { ... },
  "error": "message if failed",
  "meta": { "total": 100, "page": 1, "limit": 10 }
}
```

## Database Patterns

### Avoiding N+1

```
// BAD: N queries
for item in items:
    item.user = get_user(item.user_id)

// GOOD: 1 query
user_ids = [item.user_id for item in items]
users = get_users_by_ids(user_ids)
user_map = {u.id: u for u in users}
for item in items:
    item.user = user_map[item.user_id]
```

### Transaction Handling

```
// Pattern: Wrap related operations
begin_transaction()
try:
    operation_1()
    operation_2()
    commit()
except:
    rollback()
    raise
```

## Authentication Pattern

```
// Middleware pattern (pseudo-code)
function auth_middleware(request):
    token = request.headers.get("Authorization")?.replace("Bearer ", "")
    if not token:
        throw Unauthorized("Missing token")
    
    try:
        payload = verify_jwt(token)
        request.user = payload
    except:
        throw Unauthorized("Invalid token")
```

## Security Checklist

- [ ] Input validation on all endpoints
- [ ] Rate limiting
- [ ] Authentication required (unless public)
- [ ] Authorization checks
- [ ] No hardcoded secrets
- [ ] Parameterized queries only
- [ ] Error messages don't leak internals
