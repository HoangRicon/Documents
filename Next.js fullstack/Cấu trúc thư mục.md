# NEXTJS_FULLSTACK_FOLDER_STRUCTURE

## Cấu trúc thư mục

```bash
my-app/
├── src/
│   ├── app/
│   │   ├── (public)/
│   │   │   ├── page.tsx                      # Trang chủ public (/)
│   │   │   ├── about/
│   │   │   │   └── page.tsx                  # Trang giới thiệu (/about)
│   │   │   └── contact/
│   │   │       └── page.tsx                  # Trang liên hệ (/contact)
│   │   │
│   │   ├── (auth)/
│   │   │   ├── login/
│   │   │   │   └── page.tsx                  # Trang đăng nhập (/login)
│   │   │   ├── register/
│   │   │   │   └── page.tsx                  # Trang đăng ký (/register)
│   │   │   └── forgot-password/
│   │   │       └── page.tsx                  # Trang quên mật khẩu (/forgot-password)
│   │   │
│   │   ├── admin/
│   │   │   ├── layout.tsx                    # Layout riêng cho admin
│   │   │   ├── page.tsx                      # Dashboard admin (/admin)
│   │   │   ├── users/
│   │   │   │   ├── page.tsx                  # Quản lý người dùng (/admin/users)
│   │   │   │   └── [id]/
│   │   │   │       └── page.tsx              # Chi tiết user (/admin/users/[id])
│   │   │   ├── orders/
│   │   │   │   ├── page.tsx                  # Quản lý đơn hàng (/admin/orders)
│   │   │   │   └── [id]/
│   │   │   │       └── page.tsx              # Chi tiết order (/admin/orders/[id])
│   │   │   ├── products/
│   │   │   │   ├── page.tsx                  # Quản lý sản phẩm (/admin/products)
│   │   │   │   ├── new/
│   │   │   │   │   └── page.tsx              # Tạo sản phẩm mới (/admin/products/new)
│   │   │   │   └── [id]/
│   │   │   │       ├── page.tsx              # Chi tiết sản phẩm (/admin/products/[id])
│   │   │   │       └── edit/
│   │   │   │           └── page.tsx          # Sửa sản phẩm (/admin/products/[id]/edit)
│   │   │   └── settings/
│   │   │       └── page.tsx                  # Cài đặt hệ thống (/admin/settings)
│   │   │
│   │   ├── api/
│   │   │   ├── auth/
│   │   │   │   ├── login/
│   │   │   │   │   └── route.ts              # POST /api/auth/login
│   │   │   │   ├── register/
│   │   │   │   │   └── route.ts              # POST /api/auth/register
│   │   │   │   └── logout/
│   │   │   │       └── route.ts              # POST /api/auth/logout
│   │   │   ├── users/
│   │   │   │   ├── route.ts                  # GET/POST /api/users
│   │   │   │   └── [id]/
│   │   │   │       └── route.ts              # GET/PUT/DELETE /api/users/[id]
│   │   │   ├── orders/
│   │   │   │   ├── route.ts                  # GET/POST /api/orders
│   │   │   │   └── [id]/
│   │   │   │       └── route.ts              # GET/PUT/DELETE /api/orders/[id]
│   │   │   └── products/
│   │   │       ├── route.ts                  # GET/POST /api/products
│   │   │       └── [id]/
│   │   │           └── route.ts              # GET/PUT/DELETE /api/products/[id]
│   │   │
│   │   ├── layout.tsx                        # Root layout toàn app
│   │   ├── page.tsx                          # Landing page (/)
│   │   ├── not-found.tsx                     # Trang 404
│   │   ├── error.tsx                         # Error boundary
│   │   ├── loading.tsx                       # Loading UI
│   │   └── globals.css                       # CSS global
│   │
│   ├── components/
│   │   ├── ui/                               # shadcn/ui components
│   │   │   ├── button.tsx
│   │   │   ├── input.tsx
│   │   │   ├── card.tsx
│   │   │   └── ...
│   │   │
│   │   └── shared/                           # Shared business components
│   │       ├── header.tsx
│   │       ├── footer.tsx
│   │       ├── sidebar.tsx
│   │       └── data-table.tsx
│   │
│   ├── features/
│   │   ├── auth/
│   │   │   ├── components/                   # Auth-specific components
│   │   │   │   ├── login-form.tsx
│   │   │   │   └── register-form.tsx
│   │   │   ├── actions.ts                    # Server actions cho auth
│   │   │   └── schemas.ts                    # Zod schemas cho auth
│   │   │
│   │   ├── users/
│   │   │   ├── components/                   # User-specific components
│   │   │   │   ├── user-list.tsx
│   │   │   │   ├── user-form.tsx
│   │   │   │   └── user-profile.tsx
│   │   │   ├── actions.ts                    # Server actions cho users
│   │   │   └── schemas.ts                    # Zod schemas cho users
│   │   │
│   │   ├── orders/
│   │   │   ├── components/
│   │   │   │   ├── order-list.tsx
│   │   │   │   ├── order-form.tsx
│   │   │   │   └── order-detail.tsx
│   │   │   ├── actions.ts
│   │   │   └── schemas.ts
│   │   │
│   │   └── products/
│   │       ├── components/
│   │       │   ├── product-list.tsx
│   │       │   ├── product-form.tsx
│   │       │   └── product-card.tsx
│   │       ├── actions.ts
│   │       └── schemas.ts
│   │
│   ├── lib/
│   │   ├── auth.ts                           # Auth helpers và session
│   │   ├── permissions.ts                    # Permission helpers
│   │   ├── db.ts                             # Database client export
│   │   ├── utils.ts                          # Utility functions
│   │   ├── constants.ts                      # App constants
│   │   ├── env.ts                            # Environment validation
│   │   └── validations.ts                    # Shared validation helpers
│   │
│   ├── server/
│   │   ├── db/
│   │   │   ├── prisma.ts                     # Prisma client singleton
│   │   │   └── repositories/
│   │   │       ├── user.repository.ts
│   │   │       ├── order.repository.ts
│   │   │       └── product.repository.ts
│   │   │
│   │   ├── services/
│   │   │   ├── user.service.ts               # Business logic cho users
│   │   │   ├── order.service.ts              # Business logic cho orders
│   │   │   └── product.service.ts            # Business logic cho products
│   │   │
│   │   └── guards/
│   │       ├── require-auth.ts               # Auth guard
│   │       ├── require-admin.ts              # Admin guard
│   │       └── require-role.ts               # Role-based guard
│   │
│   ├── types/
│   │   ├── index.ts                          # Global type exports
│   │   ├── api.ts                            # API response types
│   │   └── models.ts                         # Domain model types
│   │
│   ├── hooks/                                # Custom React hooks
│   │   ├── use-user.ts
│   │   └── use-toast.ts
│   │
│   ├── providers/                            # React context providers
│   │   └── theme-provider.tsx
│   │
│   └── middleware.ts                         # Next.js middleware
│
├── prisma/
│   ├── schema.prisma                         # Database schema
│   ├── migrations/                           # Migration files
│   └── seed.ts                               # Seed script
│
├── public/                                   # Static assets
│   ├── images/
│   └── icons/
│
├── tests/
│   ├── unit/                                 # Unit tests
│   ├── integration/                          # Integration tests
│   └── e2e/                                  # E2E tests
│
├── .env                                      # Local environment variables
├── .env.example                              # Environment template
├── next.config.ts                            # Next.js config
├── package.json
├── tsconfig.json
├── tailwind.config.ts                        # Tailwind config
├── components.json                           # shadcn/ui config
└── README.md
```
│   │   ├── auth.ts                       # Auth helpers và session
│   │   ├── permissions.ts                # Permission helpers
│   │   ├── db.ts                         # Database client export
│   │   ├── utils.ts                      # Utility functions
│   │   ├── constants.ts                  # App constants
│   │   ├── env.ts                        # Environment validation
│   │   └── validations.ts                # Shared validation helpers
│   │
│   ├── server/
│   │   ├── db/
│   │   │   ├── prisma.ts                 # Prisma client singleton
│   │   │   └── repositories/
│   │   │       ├── user.repository.ts
│   │   │       ├── order.repository.ts
│   │   │       └── product.repository.ts
│   │   │
│   │   ├── services/
│   │   │   ├── user.service.ts           # Business logic cho users
│   │   │   ├── order.service.ts          # Business logic cho orders
│   │   │   └── product.service.ts        # Business logic cho products
│   │   │
│   │   └── guards/
│   │       ├── require-auth.ts           # Auth guard
│   │       ├── require-admin.ts          # Admin guard
│   │       └── require-role.ts           # Role-based guard
│   │
│   ├── types/
│   │   ├── index.ts                      # Global type exports
│   │   ├── api.ts                        # API response types
│   │   └── models.ts                     # Domain model types
│   │
│   ├── hooks/                            # Custom React hooks
│   │   ├── use-user.ts
│   │   └── use-toast.ts
│   │
│   └── providers/                        # React context providers
│       └── theme-provider.tsx
│
├── middleware.ts                         # Next.js middleware (root level)
├── prisma/
│   ├── schema.prisma                     # Database schema
│   ├── migrations/                       # Migration files
│   └── seed.ts                           # Seed script
│
├── public/                               # Static assets
│   ├── images/
│   └── icons/
│
├── tests/
│   ├── unit/                             # Unit tests
│   ├── integration/                      # Integration tests
│   └── e2e/                              # E2E tests
│
├── .env                                  # Local environment variables
├── .env.example                          # Environment template
├── next.config.ts                        # Next.js config
├── package.json
├── tsconfig.json
├── tailwind.config.ts                    # Tailwind config
├── components.json                       # shadcn/ui config
└── README.md
```

## Giải thích chi tiết

### 1. `src/app/` - App Router
Chứa toàn bộ route, layout, page của Next.js App Router. **Chỉ chứa UI composition, không chứa business logic.**

#### Route Groups (dùng dấu ngoặc đơn)
- `(public)/` - Route công khai cho tất cả user (đã đăng nhập hoặc chưa)
  - Trang chủ, About, Contact
  - Không cần authentication
- `(auth)/` - Route xác thực
  - Login, Register, Forgot Password
  - Có thể có layout riêng cho auth pages

#### Admin Routes
- `admin/` - Route quản trị
  - Có layout riêng với admin sidebar/header
  - Bắt buộc có admin guard

#### API Routes
- `api/` - API routes với cấu trúc RESTful chuẩn
- Mỗi resource có folder riêng với `route.ts` và `[id]/route.ts`

#### Special Files
- `layout.tsx` - Root layout áp dụng cho toàn app
- `page.tsx` - Landing page (/)
- `not-found.tsx` - Custom 404 page
- `error.tsx` - Error boundary
- `loading.tsx` - Loading UI (Suspense fallback)
- `globals.css` - Global styles

### 2. `src/components/` - UI Layer
**Chỉ chứa UI components, không chứa business logic.**

- `ui/` - shadcn/ui components (Button, Input, Card...)
- `shared/` - Shared business components (Header, Footer, DataTable...)

### 3. `src/features/` - Feature Layer
**Tổ chức code theo domain nghiệp vụ.** Mỗi feature chứa:

- `components/` - Components đặc thù cho feature đó
- `actions.ts` - Server actions (Next.js server actions)
- `schemas.ts` - Zod validation schemas

**Lưu ý:** Không tạo thêm `services/` trong features vì business logic nằm ở `server/services/`

### 4. `src/lib/` - Shared Utilities
Code dùng chung toàn app, không phụ thuộc vào domain cụ thể.

### 5. `src/server/` - Server-Only Layer
**Chỉ chạy phía server, không được import vào client components.**

- `db/prisma.ts` - Prisma client singleton
- `db/repositories/` - Data access layer (chỉ query DB)
- `services/` - Business logic layer (xử lý nghiệp vụ)
- `guards/` - Auth và permission guards

### 6. `src/types/` - Type Definitions
Global TypeScript types dùng chung.

### 7. `src/hooks/` - Custom React Hooks
Client-side React hooks.

### 8. `src/providers/` - React Context Providers
React context providers (Theme, Auth, etc.)

### 9. `src/middleware.ts` - Middleware
Next.js middleware cho auth và routing logic.

## Nguyên tắc tổ chức

### Boundary Rules (Quy tắc ranh giới)
1. **src/app/** → Chỉ route và UI composition
2. **src/components/** → Chỉ UI, không business logic
3. **src/features/** → Components + schemas + actions theo domain
4. **src/server/services/** → Business logic tập trung
5. **src/server/repositories/** → Chỉ truy cập DB
6. **src/lib/** → Utilities không phụ thuộc domain
7. **src/middleware.ts** → Auth và routing logic

### Import Rules (Quy tắc import)
- ✅ `src/app/` có thể import từ `src/components/`, `src/features/`, `src/lib/`
- ✅ `src/features/actions.ts` có thể import từ `src/server/services/`, `src/server/guards/`
- ✅ `src/server/services/` có thể import từ `src/server/repositories/`
- ❌ `src/components/` KHÔNG được import từ `src/server/`
- ❌ `src/server/` KHÔNG được import từ `src/features/components/`
- ❌ Client components KHÔNG được import server-only code

### Data Flow (Luồng dữ liệu)
```
Page (src/app/)
  ↓
Server Action (src/features/*/actions.ts)
  ↓
Guard (src/server/guards/)
  ↓
Service (src/server/services/)
  ↓
Repository (src/server/db/repositories/)
  ↓
Database (Prisma)
```

### Naming Conventions
- Files: `kebab-case.tsx`, `kebab-case.ts`
- Components: `PascalCase`
- Functions: `camelCase`
- Constants: `UPPER_SNAKE_CASE`
- Types/Interfaces: `PascalCase`

### App Router Conventions
- Route groups: `(group-name)/` - không tạo URL segment
- Dynamic routes: `[id]/` - tạo dynamic segment
- Catch-all: `[...slug]/` - catch tất cả segments
- Optional catch-all: `[[...slug]]/`

## So sánh với cấu trúc cũ

### Cải tiến chính:
1. **Tất cả trong src/**: `app/`, `components/`, `features/`, `lib/`, `server/` đều trong `src/`
2. **Route groups đơn giản**: Chỉ `(public)` và `(auth)`, không cần `(dashboard)`
3. **Public cho tất cả**: `(public)` dùng cho cả user chưa đăng nhập và đã đăng nhập
4. **Dynamic routes**: Sử dụng `[id]/page.tsx` cho chi tiết
5. **Nested routes**: Ví dụ `/admin/products/[id]/edit`
6. **API routes chuẩn**: Mỗi resource có `route.ts` và `[id]/route.ts`

## Ví dụ cụ thể

### Feature: Products

#### Cấu trúc files:
```
src/features/products/
├── components/
│   ├── product-list.tsx      # Client component hiển thị danh sách
│   ├── product-form.tsx      # Client component form
│   └── product-card.tsx      # Client component card
├── actions.ts                # Server actions (createProduct, updateProduct...)
└── schemas.ts                # Zod schemas (productSchema, updateProductSchema...)

src/server/services/
└── product.service.ts        # Business logic (validation, transformation...)

src/server/db/repositories/
└── product.repository.ts     # Data access (CRUD operations)

app/admin/products/
├── page.tsx                  # List page (/admin/products)
├── new/
│   └── page.tsx              # Create page (/admin/products/new)
└── [id]/
    ├── page.tsx              # Detail page (/admin/products/[id])
    └── edit/
        └── page.tsx          # Edit page (/admin/products/[id]/edit)

app/api/products/
├── route.ts                  # GET/POST /api/products
└── [id]/
    └── route.ts              # GET/PUT/DELETE /api/products/[id]
```

### Page Example: `src/app/admin/products/page.tsx`
```typescript
import { ProductList } from "@/features/products/components/product-list";
import { getProducts } from "@/features/products/actions";

export default async function ProductsPage() {
  const products = await getProducts();

  return (
    <div>
      <h1>Products</h1>
      <ProductList products={products} />
    </div>
  );
}
```

### Server Action: `src/features/products/actions.ts`
```typescript
"use server";

import { requireAdminGuard } from "@/server/guards/require-admin";
import { productService } from "@/server/services/product.service";
import { productSchema } from "./schemas";

export async function createProduct(data: unknown) {
  await requireAdminGuard(); // Guard
  const validated = productSchema.parse(data); // Validation
  return productService.createProduct(validated); // Service
}

export async function getProducts() {
  await requireAdminGuard();
  return productService.getProducts({ page: 1, limit: 10 });
}
```

### API Route: `src/app/api/products/[id]/route.ts`
```typescript
import { requireAdminGuard } from "@/server/guards/require-admin";
import { productService } from "@/server/services/product.service";
import { NextRequest } from "next/server";

export async function GET(
  req: NextRequest,
  { params }: { params: { id: string } }
) {
  await requireAdminGuard(); // Guard
  const product = await productService.getProductById(params.id); // Service
  return Response.json(product);
}

export async function PUT(
  req: NextRequest,
  { params }: { params: { id: string } }
) {
  await requireAdminGuard();
  const data = await req.json();
  const product = await productService.updateProduct(params.id, data);
  return Response.json(product);
}

export async function DELETE(
  req: NextRequest,
  { params }: { params: { id: string } }
) {
  await requireAdminGuard();
  await productService.deleteProduct(params.id);
  return Response.json({ success: true });
}
```

### Route Groups Example

#### Public routes (cho tất cả user):
```
src/app/(public)/
├── page.tsx          → /
├── about/
│   └── page.tsx      → /about
└── contact/
    └── page.tsx      → /contact
```

#### Auth routes:
```
src/app/(auth)/
├── login/
│   └── page.tsx      → /login
└── register/
    └── page.tsx      → /register
```

#### Admin routes (cần admin role):
```
src/app/admin/
├── layout.tsx        → Shared layout với admin sidebar
├── page.tsx          → /admin
├── users/
│   ├── page.tsx      → /admin/users
│   └── [id]/
│       └── page.tsx  → /admin/users/[id]
└── products/
    ├── page.tsx      → /admin/products
    ├── new/
    │   └── page.tsx  → /admin/products/new
    └── [id]/
        ├── page.tsx  → /admin/products/[id]
        └── edit/
            └── page.tsx → /admin/products/[id]/edit
```

## Checklist khi thêm feature mới

- [ ] Tạo folder trong `src/features/[feature-name]/`
- [ ] Tạo `components/` cho UI components
- [ ] Tạo `schemas.ts` cho Zod validation
- [ ] Tạo `actions.ts` cho server actions
- [ ] Tạo service trong `src/server/services/[feature].service.ts`
- [ ] Tạo repository trong `src/server/db/repositories/[feature].repository.ts`
- [ ] Thêm model vào `prisma/schema.prisma`
- [ ] Tạo API routes trong `src/app/api/[feature]/`
- [ ] Thêm pages trong `src/app/(public)/` hoặc `src/app/admin/`
- [ ] Viết tests trong `tests/`

## Lưu ý quan trọng

### 1. Vị trí folders
- ✅ Tất cả trong `src/`: app, components, features, lib, server, types, hooks, providers, middleware.ts
- ✅ Ngoài `src/`: prisma, public, tests, config files

### 2. Route Groups
- `(public)` - Dùng cho tất cả user (đã đăng nhập hoặc chưa)
- `(auth)` - Chỉ cho auth pages (login, register)
- `admin/` - Không dùng route group, là route thật với guard

### 3. Server vs Client Components
- Mặc định tất cả components trong `src/app/` là Server Components
- Chỉ thêm `"use client"` khi cần:
  - State (useState, useReducer)
  - Effects (useEffect)
  - Event handlers (onClick, onChange)
  - Browser APIs (window, document)
  - Custom hooks

### 4. Server Actions
- Phải có `"use server"` ở đầu file hoặc function
- Chỉ được gọi từ Server Components hoặc Client Components
- Tự động serialize data giữa client và server

### 5. Route Handlers (API Routes)
- File phải tên là `route.ts` (không phải `api.ts` hay `handler.ts`)
- Export các HTTP methods: GET, POST, PUT, DELETE, PATCH
- Nhận `NextRequest` và trả về `Response`