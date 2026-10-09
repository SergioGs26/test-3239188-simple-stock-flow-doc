# Domain Glossary — Simple Stock Flow

## 1. Purpose

This glossary defines the main business terms used in Simple Stock Flow. It helps the team use the same language when discussing requirements, domain rules, and the data model.

The definitions are based on `spec/data-model.md`, Section 1.

## 2. Business Terms

| Term                       | Definition                                                                                                                                                     | Technical Reference               |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| **Product**                | An item available in the catalog. It has a name, price, stock quantity, category, and optional image.                                                          | `Product` / `product`             |
| **Category**               | A classification assigned to a product. The system uses a fixed set of five categories.                                                                        | `Category` / `category`           |
| **Price**                  | The current monetary value of a product in the catalog. It must be greater than zero.                                                                          | `Money` / `product.price`         |
| **Stock**                  | The number of units currently available for a product. It cannot be negative.                                                                                  | `product.stock`                   |
| **Product Image**          | An optional reference key to an image stored externally. The database stores the key, not the image file itself.                                               | `product.image_key`               |
| **Sale**                   | A completed commercial transaction. It records when the sale occurred and who registered it. A registered sale is not edited or deleted.                       | `Sale` / `sale`                   |
| **Sale Item**              | A line within a sale containing a product reference, quantity, and the unit price captured at the time of the sale. It cannot exist independently of its sale. | `SaleItem` / `sale_item`          |
| **Quantity**               | The number of units of a product included in a sale item. It must be greater than zero.                                                                        | `Quantity` / `sale_item.quantity` |
| **Unit Price Snapshot**    | A copy of the product's price at the time of the sale. It remains unchanged if the catalog price changes later.                                                | `sale_item.unit_price`            |
| **Product Name Snapshot**  | A copy of the product name at the time of the sale. It preserves the information associated with the original transaction.                                     | `sale_item.product_name`          |
| **Category Name Snapshot** | A copy of the category name associated with the sale item at the time of the sale. It preserves historical information if the catalog category is renamed.     | `sale_item.category_name`         |
| **Line Subtotal**          | The result of multiplying the unit price snapshot by the quantity. It is calculated and is not stored as a separate column.                                    | `SaleItem.Subtotal`               |
| **Sale Total**             | The sum of the subtotals of all sale items in a sale. It is calculated and is not stored as a separate column.                                                 | `Sale.Total`                      |
| **User**                   | An internal operator who authenticates in the system and can register sales. The model does not define a customer or buyer entity.                             | `User` / `user`                   |
| **Role**                   | The type of access assigned to a user. The defined roles are `admin` and `seller`.                                                                             | `user.role`                       |
| **Password Hash**          | A protected representation of a password. The domain must not receive or store the original plain-text password.                                               | `user.password_hash`              |
| **Logical Deletion**       | A way to mark a product as deleted without physically removing its database record.                                                                            | `product.deleted_at`              |
| **Date Range**             | A time interval used to filter a sales report. The end date cannot be earlier than the start date.                                                             | Application value object          |
| **Sales Report**           | A calculated view of sales grouped by product for a selected date range. It is not stored as a separate database entity.                                       | Read model / database read port   |

## 3. Important Domain Concepts

### 3.1 Snapshots and Historical Information

A snapshot is a copy of a value taken at the time a sale is registered. Sale items keep the relevant product information so that later catalog changes do not rewrite the historical transaction.

For example, changing a product's current price must not change the unit price recorded in an earlier sale.

### 3.2 Stock Management

Stock represents available units, not the number of units sold. A stock withdrawal cannot exceed the available quantity, and the resulting stock must never be negative.

Registering a sale item and withdrawing its stock must be handled as one domain operation.

### 3.3 Calculated Values

The line subtotal and sale total are calculated from the stored sale item data. They are not separate persisted fields in the defined model.

The sales report is also calculated for a requested date range rather than stored as another business entity.

### 3.4 Product and Category Information

Each product belongs to a category. Categories come from a fixed set of five entries and are not managed as ordinary catalog records.

A product may have an optional image reference. The reference is an opaque key, not a file path or the image binary itself.

## 4. Naming Conventions

The domain uses English names for entities and technical elements.

* Entity names use singular `PascalCase`, such as `Product`, `Sale`, and `SaleItem`.
* Database table names use singular `snake_case`, such as `product`, `sale`, and `sale_item`.
* Attribute and column names use `snake_case`, such as `unit_price` and `password_hash`.

The database schema is named `sales`. This is separate from the singular table names.

## 5. Source and Traceability

This glossary is based on `spec/data-model.md`:

* **Section 0:** Schema naming conventions.
* **Section 1:** Domain glossary.
* **Section 2:** Entities and their invariants.
* **Section 3:** Physical data model.
* **Section 7:** Privacy and retention.
* **Section 8:** Audit considerations.

If a definition conflicts with the current data model, the data model must be reviewed before changing the glossary.

## 6. Assumptions and Limitations

* This glossary describes the documented domain and does not claim that every planned rule is enforced directly by the database.
* Technical references identify the corresponding domain object, table, column, or application concept.
* No customer entity, persisted sales-report table, or separate currency column is defined in the current data model.
* The glossary should be updated when an approved change modifies the domain terminology or data model.
