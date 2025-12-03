# Drizzle ORM Development Assistant

You are an expert in Drizzle ORM with deep knowledge of schema management, migrations, type safety, and modern database development patterns.

## Memory Integration

This CLAUDE.md file follows official Claude Code memory management patterns:

- **Project memory** - Shared with team via source control
- **Hierarchical loading** - Builds upon user and enterprise memory
- **Import support** - Can reference additional files with @path/to/file
- **Auto-discovery** - Loaded when Claude reads files in this subtree

## Available Commands

Use these project-specific slash commands:

- `/drizzle-schema [table-name]` - Generate type-safe schema
- `/drizzle-migrate [action]` - Handle migrations  
- `/drizzle-query [type]` - Create optimized queries
- `/drizzle-seed [table]` - Generate seed data

## Project Context

This project uses **Drizzle ORM v0.44.6+** for type-safe database operations with:

- **TypeScript-first** approach with full type inference
- **SQL-like syntax** that's familiar and powerful
- **Multiple database support** - PostgreSQL 17+, MySQL 8+, SQLite
- **Identity columns** preferred over serial types (PostgreSQL)
- **Enhanced migrations** with DDL improvements
- **Gel dialect support** for PostgreSQL-compatible databases
- **Performance optimized** with prepared statements
- **Edge runtime compatible** - Works with serverless

## 🚀 2025 Drizzle ORM Updates

### Major Changes
- **Identity columns over serial** for PostgreSQL (recommended pattern)
- **Enhanced DDL generation** with stricter validation
- **New Gel dialect** for PostgreSQL-compatible databases
- **Improved kit behavior** - no more IF NOT EXISTS in DDL
- **Better version compatibility** between ORM and Kit
- **PostgreSQL 17+ support** with latest features

## Core Drizzle Principles

### 1. Schema Definition

- **Define schemas declaratively** using Drizzle's schema builders
- **Use proper types** for each column with validation
- **Establish relationships** with foreign keys and references
- **Index strategically** for query performance
- **Version schemas** with proper migration patterns

### 2. Type Safety

- **Full TypeScript inference** from schema to queries
- **Compile-time validation** of SQL operations
- **IntelliSense support** for table columns and relations
- **Runtime validation** with Drizzle's built-in validators
- **Type-safe joins** and complex queries

### 3. Migration Management

- **Generate migrations** automatically from schema changes
- **Version control migrations** with proper naming
- **Run migrations safely** in development and production
- **Rollback support** for schema changes
- **Seed data management** for consistent environments

## Database Setup

### PostgreSQL with Neon

```typescript
// lib/db.ts
import { drizzle } from 'drizzle-orm/neon-http';
import { neon, neonConfig } from '@neondatabase/serverless';

neonConfig.fetchConnectionCache = true;

const sql = neon(process.env.DATABASE_URL!);
export const db = drizzle(sql);
```

### SQLite for Local Development

```typescript
// lib/db.ts
import { drizzle } from 'drizzle-orm/better-sqlite3';
import Database from 'better-sqlite3';

const sqlite = new Database('./dev.db');
export const db = drizzle(sqlite);
```

### MySQL with PlanetScale

```typescript
// lib/db.ts
import { drizzle } from 'drizzle-orm/mysql2';
import mysql from 'mysql2/promise';

const connection = mysql.createPool({
  uri: process.env.DATABASE_URL,
});

export const db = drizzle(connection);
```

## Schema Patterns

### User Management Schema (2025 Pattern)

```typescript
// schema/users.ts - Using identity columns (recommended)
import { pgTable, integer, text, timestamp, boolean } from 'drizzle-orm/pg-core';

export const users = pgTable('users', {
  // ✅ RECOMMENDED: Use identity() instead of serial()
  id: integer('id').primaryKey().generatedByDefaultAsIdentity(),
  email: text('email').notNull().unique(),
  name: text('name').notNull(),
  avatar: text('avatar'),
  emailVerified: boolean('email_verified').default(false),
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow(),
});

export type User = typeof users.$inferSelect;
export type NewUser = typeof users.$inferInsert;

// Legacy pattern (deprecated but still supported)
// import { pgTable, serial } from 'drizzle-orm/pg-core';
// export const usersLegacy = pgTable('users_legacy', {
//   id: serial('id').primaryKey(), // ⚠️ Use identity() instead
// });
```

### Content with Relations (2025 Pattern)

```typescript
// schema/posts.ts - Using identity columns and enhanced relations
import { pgTable, integer, text, timestamp, boolean } from 'drizzle-orm/pg-core';
import { relations } from 'drizzle-orm';
import { users } from './users';

export const posts = pgTable('posts', {
  // ✅ Use identity column for primary key
  id: integer('id').primaryKey().generatedByDefaultAsIdentity(),
  title: text('title').notNull(),
  content: text('content').notNull(),
  slug: text('slug').notNull().unique(),
  published: boolean('published').default(false),
  // Foreign key with proper cascading
  authorId: integer('author_id').references(() => users.id, {
    onDelete: 'cascade',
    onUpdate: 'cascade',
  }),
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow(),
});

export const postsRelations = relations(posts, ({ one }) => ({
  author: one(users, {
    fields: [posts.authorId],
    references: [users.id],
  }),
}));

export const usersRelations = relations(users, ({ many }) => ({
  posts: many(posts),
}));

export type Post = typeof posts.$inferSelect;
export type NewPost = typeof posts.$inferInsert;
```

### E-commerce Schema

```typescript
// schema/ecommerce.ts
import { pgTable, serial, text, integer, decimal, timestamp } from 'drizzle-orm/pg-core';

export const products = pgTable('products', {
  id: serial('id').primaryKey(),
  name: text('name').notNull(),
  description: text('description'),
  price: decimal('price', { precision: 10, scale: 2 }).notNull(),
  stock: integer('stock').default(0),
  sku: text('sku').notNull().unique(),
  categoryId: integer('category_id').references(() => categories.id),
  createdAt: timestamp('created_at').defaultNow(),
});

export const categories = pgTable('categories', {
  id: serial('id').primaryKey(),
  name: text('name').notNull(),
  slug: text('slug').notNull().unique(),
  description: text('description'),
});

export const orders = pgTable('orders', {
  id: serial('id').primaryKey(),
  userId: integer('user_id').references(() => users.id),
  total: decimal('total', { precision: 10, scale: 2 }).notNull(),
  status: text('status', { enum: ['pending', 'processing', 'shipped', 'delivered', 'cancelled'] }).default('pending'),
  createdAt: timestamp('created_at').defaultNow(),
});

export const orderItems = pgTable('order_items', {
  id: serial('id').primaryKey(),
  orderId: integer('order_id').references(() => orders.id),
  productId: integer('product_id').references(() => products.id),
  quantity: integer('quantity').notNull(),
  price: decimal('price', { precision: 10, scale: 2 }).notNull(),
});
```

## Query Patterns

### Basic CRUD Operations

```typescript
// lib/queries/users.ts
import { db } from '@/lib/db';
import { users } from '@/schema/users';
import { eq, desc, count } from 'drizzle-orm';

// Create user
export async function createUser(userData: NewUser) {
  const [user] = await db.insert(users).values(userData).returning();
  return user;
}

// Get user by ID
export async function getUserById(id: number) {
  const user = await db.select().from(users).where(eq(users.id, id));
  return user[0];
}

// Get user by email
export async function getUserByEmail(email: string) {
  const user = await db.select().from(users).where(eq(users.email, email));
  return user[0];
}

// Update user
export async function updateUser(id: number, userData: Partial<NewUser>) {
  const [user] = await db
    .update(users)
    .set(userData)
    .where(eq(users.id, id))
    .returning();
  return user;
}

// Delete user
export async function deleteUser(id: number) {
  await db.delete(users).where(eq(users.id, id));
}

// Get paginated users
export async function getPaginatedUsers(page = 1, limit = 10) {
  const offset = (page - 1) * limit;
  
  const [userList, totalCount] = await Promise.all([
    db.select().from(users).limit(limit).offset(offset).orderBy(desc(users.createdAt)),
    db.select({ count: count() }).from(users),
  ]);
  
  return {
    users: userList,
    total: totalCount[0].count,
    page,
    totalPages: Math.ceil(totalCount[0].count / limit),
  };
}
```

### Complex Relations

```typescript
// lib/queries/posts.ts
import { db } from '@/lib/db';
import { posts, users } from '@/schema';
import { eq, desc, and, ilike } from 'drizzle-orm';

// Get posts with authors
export async function getPostsWithAuthors() {
  return await db
    .select({
      id: posts.id,
      title: posts.title,
      content: posts.content,
      published: posts.published,
      createdAt: posts.createdAt,
      author: {
        id: users.id,
        name: users.name,
        email: users.email,
      },
    })
    .from(posts)
    .innerJoin(users, eq(posts.authorId, users.id))
    .where(eq(posts.published, true))
    .orderBy(desc(posts.createdAt));
}

// Search posts
export async function searchPosts(query: string) {
  return await db
    .select()
    .from(posts)
    .where(
      and(
        eq(posts.published, true),
        ilike(posts.title, `%${query}%`)
      )
    )
    .orderBy(desc(posts.createdAt));
}

// Get user's posts
export async function getUserPosts(userId: number) {
  return await db
    .select()
    .from(posts)
    .where(eq(posts.authorId, userId))
    .orderBy(desc(posts.createdAt));
}
```

### Advanced Queries

```typescript
// lib/queries/analytics.ts
import { db } from '@/lib/db';
import { orders, orderItems, products, users } from '@/schema';
import { sum, count, avg, desc, gte } from 'drizzle-orm';

// Sales analytics
export async function getSalesAnalytics(startDate: Date, endDate: Date) {
  return await db
    .select({
      totalRevenue: sum(orders.total),
      totalOrders: count(orders.id),
      averageOrderValue: avg(orders.total),
    })
    .from(orders)
    .where(
      and(
        gte(orders.createdAt, startDate),
        lte(orders.createdAt, endDate)
      )
    );
}

// Top selling products
export async function getTopSellingProducts(limit = 10) {
  return await db
    .select({
      productId: products.id,
      productName: products.name,
      totalSold: sum(orderItems.quantity),
      revenue: sum(orderItems.price),
    })
    .from(orderItems)
    .innerJoin(products, eq(orderItems.productId, products.id))
    .groupBy(products.id, products.name)
    .orderBy(desc(sum(orderItems.quantity)))
    .limit(limit);
}
```

## Migration Management

### Drizzle Config (2025 Version)

```typescript
// drizzle.config.ts - Compatible with Kit v0.31.5+
import type { Config } from 'drizzle-kit';

export default {
  schema: './src/schema/*',
  out: './drizzle',
  dialect: 'postgresql', // Updated from 'driver: pg'
  dbCredentials: {
    url: process.env.DATABASE_URL!, // Updated from connectionString
  },
  verbose: true,
  strict: true,
  // New options in 2025
  migrations: {
    table: '_drizzle_migrations',
    schema: 'public',
  },
  // Enhanced introspection options
  introspect: {
    casing: 'camelCase',
  },
} satisfies Config;

// Alternative: For Gel dialect (PostgreSQL-compatible)
export const gelConfig = {
  schema: './src/schema/*',
  out: './drizzle',
  dialect: 'gel', // New Gel dialect
  dbCredentials: {
    url: process.env.GEL_DATABASE_URL!,
  },
  verbose: true,
  strict: true,
} satisfies Config;
```

### Common Commands (2025 Updated)

```bash
# Generate migration (new syntax)
npx drizzle-kit generate

# Push schema changes to database
npx drizzle-kit push

# Introspect existing database
npx drizzle-kit introspect

# Check migration status
npx drizzle-kit check

# Studio (database browser) - Enhanced UI
npx drizzle-kit studio

# New in 2025: Migration checking and validation
npx drizzle-kit validate

# Legacy syntax still supported but deprecated
# npx drizzle-kit generate:pg  # ⚠️ Deprecated
# npx drizzle-kit push:pg      # ⚠️ Deprecated
```

### Migration Scripts

```typescript
// scripts/migrate.ts
import { drizzle } from 'drizzle-orm/neon-http';
import { migrate } from 'drizzle-orm/neon-http/migrator';
import { neon } from '@neondatabase/serverless';

const sql = neon(process.env.DATABASE_URL!);
const db = drizzle(sql);

async function runMigrations() {
  console.log('Running migrations...');
  await migrate(db, { migrationsFolder: 'drizzle' });
  console.log('Migrations completed!');
  process.exit(0);
}

runMigrations().catch((err) => {
  console.error('Migration failed!', err);
  process.exit(1);
});
```

### Seed Data

```typescript
// scripts/seed.ts
import { db } from '@/lib/db';
import { users, posts, categories } from '@/schema';

async function seedDatabase() {
  console.log('Seeding database...');
  
  // Create users
  const userIds = await db.insert(users).values([
    { email: 'admin@example.com', name: 'Admin User' },
    { email: 'user@example.com', name: 'Regular User' },
  ]).returning({ id: users.id });
  
  // Create categories
  const categoryIds = await db.insert(categories).values([
    { name: 'Technology', slug: 'technology' },
    { name: 'Design', slug: 'design' },
  ]).returning({ id: categories.id });
  
  // Create posts
  await db.insert(posts).values([
    {
      title: 'Getting Started with Drizzle',
      content: 'Learn how to use Drizzle ORM...',
      slug: 'getting-started-drizzle',
      authorId: userIds[0].id,
      published: true,
    },
    {
      title: 'Database Design Best Practices',
      content: 'Tips for designing scalable databases...',
      slug: 'database-design-best-practices',
      authorId: userIds[1].id,
      published: true,
    },
  ]);
  
  console.log('Seeding completed!');
}

seedDatabase().catch(console.error);
```

## Performance Optimization

### Prepared Statements

```typescript
// lib/prepared-statements.ts
import { db } from '@/lib/db';
import { users } from '@/schema/users';
import { eq } from 'drizzle-orm';

// Prepare frequently used queries
export const getUserByIdPrepared = db
  .select()
  .from(users)
  .where(eq(users.id, placeholder('id')))
  .prepare();

export const getUserByEmailPrepared = db
  .select()
  .from(users)
  .where(eq(users.email, placeholder('email')))
  .prepare();

// Usage
const user = await getUserByIdPrepared.execute({ id: 123 });
```

### Indexes and Constraints

```typescript
// schema/optimized.ts
import { pgTable, serial, text, timestamp, index, uniqueIndex } from 'drizzle-orm/pg-core';

export const posts = pgTable('posts', {
  id: serial('id').primaryKey(),
  title: text('title').notNull(),
  slug: text('slug').notNull(),
  content: text('content').notNull(),
  authorId: integer('author_id').notNull(),
  published: boolean('published').default(false),
  createdAt: timestamp('created_at').defaultNow(),
}, (table) => ({
  // Create indexes for better query performance
  slugIdx: uniqueIndex('posts_slug_idx').on(table.slug),
  authorIdx: index('posts_author_idx').on(table.authorId),
  publishedIdx: index('posts_published_idx').on(table.published),
  createdAtIdx: index('posts_created_at_idx').on(table.createdAt),
}));
```

### Connection Pooling

```typescript
// lib/db-pool.ts
import { drizzle } from 'drizzle-orm/mysql2';
import mysql from 'mysql2/promise';

const poolConnection = mysql.createPool({
  host: process.env.DB_HOST,
  port: parseInt(process.env.DB_PORT || '3306'),
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0,
});

export const db = drizzle(poolConnection);
```

## Testing Strategies

### Test Database Setup

```typescript
// tests/setup.ts
import { drizzle } from 'drizzle-orm/better-sqlite3';
import Database from 'better-sqlite3';
import { migrate } from 'drizzle-orm/better-sqlite3/migrator';

export function createTestDb() {
  const sqlite = new Database(':memory:');
  const db = drizzle(sqlite);
  
  // Run migrations
  migrate(db, { migrationsFolder: 'drizzle' });
  
  return db;
}
```

### Query Testing

```typescript
// tests/queries.test.ts
import { describe, it, expect, beforeEach } from 'vitest';
import { createTestDb } from './setup';
import { users } from '@/schema/users';
import { createUser, getUserByEmail } from '@/lib/queries/users';

describe('User Queries', () => {
  let db: ReturnType<typeof createTestDb>;
  
  beforeEach(() => {
    db = createTestDb();
  });
  
  it('should create and retrieve user', async () => {
    const userData = {
      email: 'test@example.com',
      name: 'Test User',
    };
    
    const user = await createUser(userData);
    expect(user.email).toBe(userData.email);
    
    const retrievedUser = await getUserByEmail(userData.email);
    expect(retrievedUser).toEqual(user);
  });
});
```

## Environment Configuration

```env
# Database URLs for different environments
DATABASE_URL=postgresql://username:password@localhost:5432/myapp_development
DATABASE_URL_TEST=postgresql://username:password@localhost:5432/myapp_test
DATABASE_URL_PRODUCTION=postgresql://username:password@host:5432/myapp_production

# For Neon (serverless PostgreSQL)
DATABASE_URL=postgresql://username:password@ep-cool-darkness-123456.us-east-1.aws.neon.tech/neondb?sslmode=require

# For PlanetScale (serverless MySQL)
DATABASE_URL=mysql://username:password@host.connect.psdb.cloud/database?sslmode=require

# For local SQLite
DATABASE_URL=file:./dev.db
```

## Common Patterns

### Repository Pattern

```typescript
// lib/repositories/user-repository.ts
import { db } from '@/lib/db';
import { users, User, NewUser } from '@/schema/users';
import { eq } from 'drizzle-orm';

export class UserRepository {
  async create(userData: NewUser): Promise<User> {
    const [user] = await db.insert(users).values(userData).returning();
    return user;
  }
  
  async findById(id: number): Promise<User | undefined> {
    const user = await db.select().from(users).where(eq(users.id, id));
    return user[0];
  }
  
  async findByEmail(email: string): Promise<User | undefined> {
    const user = await db.select().from(users).where(eq(users.email, email));
    return user[0];
  }
  
  async update(id: number, userData: Partial<NewUser>): Promise<User> {
    const [user] = await db
      .update(users)
      .set(userData)
      .where(eq(users.id, id))
      .returning();
    return user;
  }
  
  async delete(id: number): Promise<void> {
    await db.delete(users).where(eq(users.id, id));
  }
}

export const userRepository = new UserRepository();
```

### Transaction Handling

```typescript
// lib/services/order-service.ts
import { db } from '@/lib/db';
import { orders, orderItems, products } from '@/schema';
import { eq, sql } from 'drizzle-orm';

export async function createOrderWithItems(
  orderData: NewOrder,
  items: Array<{ productId: number; quantity: number }>
) {
  return await db.transaction(async (tx) => {
    // Create order
    const [order] = await tx.insert(orders).values(orderData).returning();
    
    // Create order items and update product stock
    for (const item of items) {
      // Get product price
      const product = await tx
        .select({ price: products.price, stock: products.stock })
        .from(products)
        .where(eq(products.id, item.productId));
      
      if (product[0].stock < item.quantity) {
        throw new Error(`Insufficient stock for product ${item.productId}`);
      }
      
      // Create order item
      await tx.insert(orderItems).values({
        orderId: order.id,
        productId: item.productId,
        quantity: item.quantity,
        price: product[0].price,
      });
      
      // Update product stock
      await tx
        .update(products)
        .set({
          stock: sql`${products.stock} - ${item.quantity}`,
        })
        .where(eq(products.id, item.productId));
    }
    
    return order;
  });
}
```

## Resources

- [Drizzle ORM Documentation](https://orm.drizzle.team)
- [Drizzle Kit CLI](https://orm.drizzle.team/kit-docs/overview)
- [Schema Reference](https://orm.drizzle.team/docs/sql-schema-declaration)
- [Query Reference](https://orm.drizzle.team/docs/rqb)
- [Migration Guide](https://orm.drizzle.team/docs/migrations)

Remember: **Type safety first, optimize with indexes, use transactions for consistency, and prepare statements for performance!**
