# Migration `20200303004625-orgency-mvoed-to-order`

This migration has been generated by Rostislav Klein at 3/3/2020, 12:46:26 AM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
ALTER TABLE `db`.`OrderItem` DROP FOREIGN KEY `OrderItem_ibfk_3`,
DROP COLUMN `material`,
ADD COLUMN `material` varchar(191)  ;

ALTER TABLE `db`.`OrderItem` ADD FOREIGN KEY (`material`) REFERENCES `db`.`Material`(`id`) ON DELETE SET NULL ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200303001353-depluralization..20200303004625-orgency-mvoed-to-order
--- datamodel.dml
+++ datamodel.dml
@@ -6,14 +6,14 @@
 // The `url` field must contain the connection string to your DB.
 // Learn more about connection strings for your DB: https://pris.ly/connection-strings
 datasource db {
   provider = "mysql"
-  url = "***"
+  url      = env("MYSQL_URL")
 }
 // Other examples for connection strings are:
-// SQLite: url = "***"
-// MySQL:  url = "***"
+// SQLite: url = "sqlite:./dev.db"
+// MySQL:  url = "mysql://johndoe:johndoe@localhost:3306/mydb"
 // You can also use environment variables to specify the connection string: https://pris.ly/prisma-schema#using-environment-variables
 // By adding the `generator` block, you specify that you want to generate Prisma's DB client.
 // The client is generated by runnning the `prisma generate` command and will be located in `node_modules/@prisma` and can be imported in your code as:
@@ -61,26 +61,26 @@
   totalPrice Float
   totalTax   Float
   note       String?
   items      OrderItem[]
+  urgency    Int         @default(0)
   status     OrderStatus @default(NEW)
   createdAt  DateTime    @default(now())
   updatedAt  DateTime    @updatedAt
   createdBy  User
 }
 model OrderItem {
-  id         String   @id @default(cuid())
-  material   Material
+  id         String    @id @default(cuid())
+  material   Material?
   totalPrice Float
   totalTax   Float
   name       String?
   width      Float?
   height     Float?
   pieces     Int?
-  urgency    Int      @default(0)
-  createdAt  DateTime @default(now())
-  updatedAt  DateTime @updatedAt
+  createdAt  DateTime  @default(now())
+  updatedAt  DateTime  @updatedAt
   createdBy  User
 }
 model ImagePreview {
```

