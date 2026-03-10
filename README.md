# In-Memory Data Engine - Design Patterns Project

A comprehensive Java-based in-memory database management system demonstrating the practical implementation of **7 Design Patterns** from all three GoF categories (Creational, Structural, and Behavioral).

---

## 🎯 Overview

This project implements a fully functional in-memory database engine that supports:
- Table creation and management
- CRUD operations (Create, Read, Update, Delete)
- Complex query conditions
- Data validation
- Logging system
- Table cloning
- Multiple iteration strategies

---

## 🎥 Project Presentation

For a comprehensive walkthrough of the system, including a deep dive into the Design Patterns implementation and a live demonstration of the database operations, watch the video here:

👉 **[Watch the Project Presentation](https://drive.google.com/uc?id=1rnARydEagvUA-FU0L9GtpOdqiKAaOCDk&export=download)**

---

## 🏗️ Design Patterns Implemented

### **Creational Patterns**

#### 1. **Singleton Pattern**
- **Location:** `Logger/Informer.java`, `Logger/LogManager.java`, `Logger/WriterLogger.java`
- **Purpose:** Ensures only one instance of critical system components exists
- **Implementation:**
  ```java
  LogManager logger = LogManager.getInstance();
  Informer informer = Informer.getInformer();
  WriterLogger writer = WriterLogger.getLogger();
  ```
- **Benefits:**
  - Controlled access to single instance
  - Thread-safe implementation (synchronized)
  - Lazy initialization
  - Prevents multiple logger instances causing conflicts

#### 2. **Builder Pattern**
- **Location:** `builder/TableBuilder.java`
- **Purpose:** Constructs complex Table objects step-by-step with a fluent API
- **Implementation:**
  ```java
  TableBuilder builder = new TableBuilder();
  Table table = builder.build("Users", columns);
  ```
- **Benefits:**
  - Separates object construction from representation
  - Provides clear and readable table creation process
  - Allows flexible table configuration

#### 3. **Prototype Pattern (Clone)**
- **Location:** `Clone/TableClone.java`, `models/Table.java`
- **Purpose:** Creates deep copies of Table objects without depending on concrete classes
- **Implementation:**
  ```java
  Table clonedTable = originalTable.clone();
  ```
- **Benefits:**
  - Efficient object duplication
  - Independent copies (changes don't affect original)
  - Preserves schema and data integrity

---

### **Structural Patterns**

#### 4. **Composite Pattern**
- **Location:** `Condition/` package
- **Purpose:** Builds hierarchical condition trees for complex queries
- **Implementation:**
  ```java
  Condition simple1 = new SimpleCondition("age", ">", 18);
  Condition simple2 = new SimpleCondition("name", "==", "Sara");
  Condition composite = new AndCondition(List.of(simple1, simple2));
  ```
- **Benefits:**
  - Treats individual and composite conditions uniformly
  - Enables complex logical expressions (AND, OR)
  - Easy to extend with new condition types

---

### **Behavioral Patterns**

#### 5. **Command Pattern**
- **Location:** `DataOperations/` package
- **Purpose:** Encapsulates all database operations as objects with uniform execution flow
- **Classes:**
  - `AbstractOperation` - Base command
  - `InsertOperation`, `UpdateOperation`, `DeleteOperation`
  - `QueryOperation`, `CreateTable`, `RemoveTable`
- **Implementation:**
  ```java
  AbstractOperation operation = new InsertOperation(db, "Users", rows);
  List<Row> result = operation.run();
  ```
- **Benefits:**
  - Uniform validation → execution → result flow
  - Easy to add new operations
  - Supports logging and undo functionality

#### 6. **Observer Pattern**
- **Location:** `Logger/` package
- **Purpose:** Implements a reactive logging system that responds to data changes
- **Components:**
  - `IObserver` - Observer interface
  - `IInformer` - Subject interface
  - `LogManager` - Concrete observer
  - `Informer` - Concrete subject (Singleton)
  - `WriterLogger` - File logger (Singleton)
- **Implementation:**
  ```java
  LogManager logger = LogManager.getInstance();
  Informer.getInformer().attach(logger);
  // Automatically logs all data operations
  ```
- **Benefits:**
  - Loose coupling between operations and logging
  - Easy to add new observers
  - Centralized event notification

#### 7. **Strategy Pattern**
- **Location:** `Iteration/` package
- **Purpose:** Allows dynamic selection of row iteration algorithms
- **Strategies:**
  - `ByIncomeIteration` - Returns rows in insertion order
  - `ByColumnIteration` - Returns rows sorted by column
  - `ByConditionIteration` - Returns filtered rows by condition
- **Implementation:**
  ```java
  Iiteration strategy = new ByColumnIteration("age");
  List<Row> sorted = db.iterate("Users", strategy);
  ```
- **Benefits:**
  - Interchangeable algorithms at runtime
  - Clean separation of iteration logic
  - Easy to add new iteration strategies

---

## 📁 Project Structure

```
Design Patterns Project/
├── src/
│   ├── main/
│   │   └── java/
│   │       ├── builder/           # Builder Pattern
│   │       ├── Clone/             # Prototype Pattern
│   │       ├── Condition/         # Composite Pattern
│   │       ├── DataOperations/    # Command Pattern
│   │       ├── Logger/            # Observer Pattern
│   │       ├── Iteration/         # Strategy Pattern
│   │       ├── models/            # Core data models
│   │       └── Validation/        # Validation logic
│   └── test/
│       └── java/                  # JUnit tests
├── junit-platform-console-standalone-1.9.3.jar
├── run-tests.bat
└── pom.xml
```

---

## 🔧 Prerequisites

- **Java JDK 11 or higher**
  - Download: [Oracle JDK](https://www.oracle.com/java/technologies/downloads/) or [Adoptium](https://adoptium.net/)
- **JUnit 5** (included in project)

---

## 🚀 How to Run

### **Option 1: Run Tests (Recommended)**

#### Using Command Line:

1. **Ensure Java is installed:**
   ```bash
   java -version
   javac -version
   ```

2. **Navigate to project directory:**
   ```bash
   cd "Design Patterns Project"
   ```

3. **Run the test script:**
   ```bash
   run-tests.bat
   ```

   This will:
   - Compile all source files
   - Compile test files
   - Run all JUnit tests
   - Display results

#### Manual Compilation and Testing:

```bash
# Compile source files
javac -d out src\main\java\models\*.java src\main\java\Clone\*.java src\main\java\Condition\*.java src\main\java\DataOperations\*.java src\main\java\Logger\*.java src\main\java\Validation\*.java src\main\java\Iteration\*.java src\main\java\builder\*.java

# Compile test files
javac -cp "out;junit-platform-console-standalone-1.9.3.jar" -d out src\test\java\*.java

# Run tests
java -jar junit-platform-console-standalone-1.9.3.jar --class-path out --scan-class-path
```

---

### **Option 2: Using IDE**

#### IntelliJ IDEA:
1. Open the project folder
2. Right-click on `src/test/java` → **Run 'All Tests'**

#### Eclipse:
1. Import as Maven project
2. Right-click on project → **Run As** → **JUnit Test**

#### VS Code:
1. Install Java Extension Pack
2. Open project folder
3. Click **Run Tests** in Testing panel

---

### **Option 3: Using Maven**

```bash
mvn clean test
```

---

## ✨ Features

### Core Functionality:
- ✅ Create and manage tables with typed columns
- ✅ Insert, update, delete, and query rows
- ✅ Complex condition queries (AND, OR, comparison operators)
- ✅ Schema validation
- ✅ Deep table cloning
- ✅ Automatic logging of all operations
- ✅ Multiple iteration strategies

### Data Types Supported:
- `STRING`
- `INTEGER`
- `FLOAT`
- `BOOLEAN`
- `DATE`
- `CHARACTER`

---

## 📝 Usage Examples

### Creating a Database and Table:

```java
// Create database
DataBase db = new DataBase("MyDB");

// Define columns
List<Column> columns = List.of(
    new Column("id", DataType.INTEGER),
    new Column("name", DataType.STRING),
    new Column("age", DataType.INTEGER)
);

// Create table
db.createTable("Users", columns);
```

### Inserting Data:

```java
Row row = new Row();
row.setValue("id", 1);
row.setValue("name", "Sara");
row.setValue("age", 25);

db.insertIntoTable("Users", List.of(row));
```

### Querying with Conditions:

```java
// Simple condition
Condition condition = new SimpleCondition("age", ">", 18);
List<Row> adults = db.query(condition, "Users");

// Complex condition
Condition cond1 = new SimpleCondition("age", ">", 18);
Condition cond2 = new SimpleCondition("name", "==", "Sara");
Condition combined = new AndCondition(List.of(cond1, cond2));
List<Row> result = db.query(combined, "Users");
```

### Updating Data:

```java
Row oldRow = existingRow;
Row newRow = new Row();
newRow.setValue("id", 1);
newRow.setValue("name", "Sara Updated");
newRow.setValue("age", 26);

db.updateTable("Users", List.of(oldRow), List.of(newRow));
```

### Deleting Data:

```java
db.deleteFromTable("Users", List.of(rowToDelete));
```

### Cloning a Table:

```java
Table original = db.getTable("Users");
Table cloned = original.clone();
```

### Using Iteration Strategies:

```java
// Sort by column
Iiteration sortStrategy = new ByColumnIteration("age");
List<Row> sorted = db.iterate("Users", sortStrategy);

// Filter by condition
Iiteration filterStrategy = new ByConditionIteration(condition, schema);
List<Row> filtered = db.iterate("Users", filterStrategy);
```

---

## 📊 Test Coverage

The project includes comprehensive JUnit tests:
- `InsertOperationTest` - Tests data insertion
- `UpdateOperationTest` - Tests data updates
- `DeleteOperationTest` - Tests data deletion
- `QueryOperationTest` - Tests query operations
- `CreateTableTest` - Tests table creation
- `CloneTableTest` - Tests table cloning

---

## 📄 Logging

All operations are automatically logged to `log.txt` with timestamps:

```
[LOG] [2026-03-09 00:18:52] Inserting rows into table: Users
[LOG] [2026-03-09 00:18:52] Deleted 2 rows from Users
[LOG] [2026-03-09 00:18:52] Updating rows into table: Students
```

---

## 🎓 Design Principles Applied

- **SOLID Principles**
- **Separation of Concerns**
- **Clean Code**
- **DRY (Don't Repeat Yourself)**
- **Single Responsibility Principle**

---

## 👥 Author

Design Patterns Project - In-Memory Data Engine

---

## 📜 License

This project is for educational purposes.

---

## 🔗 Additional Resources

- [Design Patterns: Elements of Reusable Object-Oriented Software](https://en.wikipedia.org/wiki/Design_Patterns)
- [Refactoring Guru - Design Patterns](https://refactoring.guru/design-patterns)
- [JUnit 5 Documentation](https://junit.org/junit5/docs/current/user-guide/)
