# Migration `20201205165754-newsreviewmigration`

This migration has been generated by Scott at 12/5/2020, 4:57:54 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "NewsAgency" (
"id" SERIAL,
    "name" TEXT NOT NULL,
    "ratingAccuracy" DECIMAL(65,30) NOT NULL
)

CREATE TABLE "Rating" (
"id" SERIAL,
    "accuracy" DECIMAL(65,30) NOT NULL,
    "bias" DECIMAL(65,30) NOT NULL
)

CREATE TABLE "reviews" (
"id" SERIAL,
    "title" TEXT NOT NULL,
    "userId" INTEGER NOT NULL,
    "createdAt" TIMESTAMP(3) NOT NULL DEFAULT CURRENT_TIMESTAMP
)

CREATE TABLE "User" (
"id" SERIAL,
    "name" TEXT NOT NULL,
    "password" TEXT,
    "createdAt" TIMESTAMP(3) NOT NULL DEFAULT CURRENT_TIMESTAMP,

    PRIMARY KEY ("id")
)

CREATE UNIQUE INDEX "NewsAgency.name_unique" ON "NewsAgency"("name")

CREATE UNIQUE INDEX "Rating.accuracy_unique" ON "Rating"("accuracy")

CREATE UNIQUE INDEX "reviews.title_unique" ON "reviews"("title")

CREATE UNIQUE INDEX "User.name_unique" ON "User"("name")

ALTER TABLE "NewsAgency" ADD FOREIGN KEY("ratingAccuracy")REFERENCES "Rating"("accuracy") ON DELETE CASCADE ON UPDATE CASCADE

ALTER TABLE "reviews" ADD FOREIGN KEY("userId")REFERENCES "User"("id") ON DELETE CASCADE ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20201205165754-newsreviewmigration
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,40 @@
+// This is your Prisma schema file,
+// learn more about it in the docs: https://pris.ly/d/prisma-schema
+
+datasource db {
+  provider = "postgresql"
+  url = "***"
+}
+
+generator client {
+  provider = "prisma-client-js"
+}
+
+model NewsAgency {
+    id Int @default(autoincrement())
+    name String @unique
+    rating Rating
+}
+
+model Rating {
+    id Int @default(autoincrement())
+    accuracy Float @unique
+    bias Float
+}
+
+model Review {
+    @@map("reviews")
+    id Int @default(autoincrement())
+    title String @unique
+    author User @relation(fields: [userId], references: [id])
+    userId Int 
+    createdAt DateTime @default(now())
+}
+
+model User {
+    id Int @id @default(autoincrement())
+    name String @unique
+    password String?
+    reviews Review[]
+    createdAt DateTime @default(now())
+}
```


