# UML Class Diagram & Design Patterns List

## 📊 Complete UML Class Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           CORE MODELS PACKAGE                               │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────┐
│   <<enumeration>>    │
│      DataType        │
├──────────────────────┤
│ + STRING             │
│ + INTEGER            │
│ + FLOAT              │
│ + BOOLEAN            │
│ + DATE               │
│ + CHARACTER          │
└──────────────────────┘

┌──────────────────────┐         ┌──────────────────────┐
│       Column         │         │        Row           │
├──────────────────────┤         ├──────────────────────┤
│ - name: String       │         │ - values: Map<String,│
│ - dataType: DataType │         │          Object>     │
├──────────────────────┤         ├──────────────────────┤
│ + getName()          │         │ + getValue(String)   │
│ + getDataType()      │         │ + setValue(String,   │
│ + setName(String)    │         │          Object)     │
│ + setDataType()      │         │ + getValues()        │
└──────────────────────┘         └──────────────────────┘

┌──────────────────────┐
│       Schema         │
├──────────────────────┤
│ - columns: List<     │
│           Column>    │
├──────────────────────┤
│ + getColumns()       │
│ + addColumn(Column)  │
└──────────────────────┘
         △
         │ contains
         │
┌────────┴─────────────────────────────────┐
│              Table                       │◄──────────┐
├──────────────────────────────────────────┤           │
│ - name: String                           │           │
│ - schema: Schema                         │           │ implements
│ - rows: List<Row>                        │           │
├──────────────────────────────────────────┤    ┌──────┴──────────┐
│ + getName()                              │    │  <<interface>>  │
│ + setName(String)                        │    │   TableClone    │
│ + getSchema()                            │    ├─────────────────┤
│ + getRows()                              │    │ + clone(): Table│
│ + addColumn(Column)                      │    └─────────────────┘
│ + addRow(Row)                            │    PROTOTYPE PATTERN
│ + removeRow(Row)                         │
│ + clone(): Table                         │
└──────────────────────────────────────────┘
         △
         │ manages
         │
┌────────┴─────────────────────────────────┐
│              DataBase                    │
├──────────────────────────────────────────┤
│ - name: String                           │
│ - tables: Map<String, Table>             │
│ - logManager: LogManager                 │
├──────────────────────────────────────────┤
│ + createTable(String, List<Column>)      │
│ + addTable(Table)                        │
│ + getTable(String)                       │
│ + insert(String, List<Row>)              │
│ + insertIntoTable(String, List<Row>)     │
│ + updateTable(String, List<Row>, ...)    │
│ + query(Condition, String)               │
│ + deleteFromTable(String, List<Row>)     │
│ + removeTable(String)                    │
│ + iterate(String, Iiteration)            │
└──────────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────────────────────┐
│                        BUILDER PATTERN PACKAGE                              │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────┐
│      <<interface>>               │
│         Builder                  │
├──────────────────────────────────┤
│ + setName(String)                │
│ + addColumn(String, DataType)    │
│ + build(String): Table           │
│ + build(String, List<Column>)    │
└──────────────────────────────────┘
         △
         │ implements
         │
┌────────┴──────────────────────────┐
│        TableBuilder               │
├───────────────────────────────────┤
│ - table: Table                    │
├───────────────────────────────────┤
│ + setName(String)                 │
│ + addColumn(String, DataType)     │
│ + build(String): Table            │
│ + build(String, List<Column>)     │
└───────────────────────────────────┘
        BUILDER PATTERN


┌─────────────────────────────────────────────────────────────────────────────┐
│                       CONDITION PACKAGE (COMPOSITE)                         │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────┐
│      <<interface>>               │
│        Condition                 │
├──────────────────────────────────┤
│ + evaluate(Row): boolean         │
│ + validate(Schema)               │
└──────────────────────────────────┘
         △
         │ implements
    ┌────┴────┬──────────────┬──────────────┐
    │         │              │              │
┌───┴────────────┐  ┌────────┴────────┐  ┌─┴──────────────┐
│SimpleCondition │  │  AndCondition   │  │  OrCondition   │
├────────────────┤  ├─────────────────┤  ├────────────────┤
│- column: String│  │- conditions:    │  │- conditions:   │
│- operator:     │  │  List<Condition>│  │  List<Condition│
│  Operator      │  ├─────────────────┤  ├────────────────┤
│- value: Object │  │+ evaluate(Row)  │  │+ evaluate(Row) │
├────────────────┤  │+ validate(...)  │  │+ validate(...) │
│+ evaluate(Row) │  └─────────────────┘  └────────────────┘
│+ validate(...) │   COMPOSITE PATTERN   COMPOSITE PATTERN
└────────────────┘

┌──────────────────────┐
│   <<enumeration>>    │
│      Operator        │
├──────────────────────┤
│ + EQUALS             │
│ + NOT_EQUALS         │
│ + GREATER_THAN       │
│ + LESS_THAN          │
│ + GREATER_THAN_OR_   │
│   EQUAL              │
│ + LESS_THAN_OR_EQUAL │
└──────────────────────┘


┌─────────────────────────────────────────────────────────────────────────────┐
│                    DATA OPERATIONS PACKAGE (COMMAND)                        │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────┐
│      <<abstract>>                        │
│      AbstractOperation                   │
├──────────────────────────────────────────┤
│ # validators: List<Validator>            │
│ # database: DataBase                     │
│ # tableName: String                      │
├──────────────────────────────────────────┤
│ + {abstract} run(): List<Row>            │
│ + {abstract} validate()                  │
│ + {abstract} execute(): List<Row>        │
└──────────────────────────────────────────┘
         △
         │ extends
    ┌────┴────┬──────────┬──────────┬──────────┬──────────┐
    │         │          │          │          │          │
┌───┴────────┐ ┌────────┴────┐ ┌───┴────────┐ ┌┴────────┐ ┌┴──────────┐
│InsertOper- │ │UpdateOper-  │ │DeleteOper- │ │Query    │ │CreateTable│
│ation       │ │ation        │ │ation       │ │Operation│ │           │
├────────────┤ ├─────────────┤ ├────────────┤ ├─────────┤ ├───────────┤
│- rows      │ │- rows       │ │- rowsTo    │ │- cond-  │ │- columns  │
│- informer  │ │- newRows    │ │  Delete    │ │  ition  │ ├───────────┤
├────────────┤ │- informer   │ │- informer  │ ├─────────┤ │+ run()    │
│+ run()     │ ├─────────────┤ ├────────────┤ │+ run()  │ │+ execute()│
│+ execute() │ │+ run()      │ │+ run()     │ │+ execute│ │+ validate │
│+ validate()│ │+ execute()  │ │+ execute() │ │+ validate│ └───────────┘
└────────────┘ │+ validate() │ │+ validate()│ └─────────┘
               └─────────────┘ └────────────┘
                                                    ┌──────────────┐
                                                    │RemoveTable   │
                                                    ├──────────────┤
                                                    │+ run()       │
                                                    │+ execute()   │
                                                    │+ validate()  │
                                                    └──────────────┘
                                                    COMMAND PATTERN


┌─────────────────────────────────────────────────────────────────────────────┐
│                    LOGGER PACKAGE (OBSERVER + SINGLETON)                    │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────┐         ┌──────────────────────────┐
│    <<interface>>         │         │    <<interface>>         │
│      IObserver           │         │      IInformer           │
├──────────────────────────┤         ├──────────────────────────┤
│ + update(String)         │         │ + attach(IObserver)      │
└──────────────────────────┘         │ + detach(IObserver)      │
         △                          │ + notifyObservers(String)│
         │ implements                └──────────────────────────┘
         │                                    △
┌────────┴──────────────┐                    │ implements
│    LogManager         │                    │
├───────────────────────┤           ┌────────┴──────────────┐
│ - instance: LogManager│◄──────────│     Informer          │
│  {static}             │  observes ├───────────────────────┤
│ - formatter           │           │ - instance: Informer  │
├───────────────────────┤           │   {static}            │
│ + getInstance()       │           │ - observers: List<    │
│   {static}            │           │   IObserver>          │
│ + update(String)      │           ├───────────────────────┤
└───────────────────────┘           │ + getInformer()       │
   SINGLETON PATTERN                │   {static}            │
                                    │ + attach(IObserver)   │
                                    │ + detach(IObserver)   │
                                    │ + notifyObservers()   │
                                    └───────────────────────┘
                                       SINGLETON PATTERN
                                       OBSERVER PATTERN
         │
         │ uses
         ▼
┌───────────────────────┐
│   WriterLogger        │
├───────────────────────┤
│ - instance:           │
│  WriterLogger {static}│
│ - writer: PrintWriter │
├───────────────────────┤
│ + getLogger() {static}│
│ + log(String)         │
│ + close()             │
└───────────────────────┘
   SINGLETON PATTERN


┌─────────────────────────────────────────────────────────────────────────────┐
│                      ITERATION PACKAGE (STRATEGY)                           │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────┐
│      <<interface>>               │
│        Iiteration                │
├──────────────────────────────────┤
│ + iterate(List<Row>): List<Row>  │
└──────────────────────────────────┘
         △
         │ implements
    ┌────┴────┬──────────────┬─────────────┐
    │         │              │             │
┌───┴────────────┐  ┌────────┴────────┐  ┌─┴──────────────┐
│ByIncomeIter-   │  │ByColumnIter-    │  │ByConditionIter-│
│ation           │  │ation            │  │ation           │
├────────────────┤  ├─────────────────┤  ├────────────────┤
│+ iterate(...)  │  │- column: String │  │- schema: Schema│
└────────────────┘  ├─────────────────┤  │- condition:    │
                    │+ iterate(...)   │  │  Condition     │
                    └─────────────────┘  ├────────────────┤
                                         │+ iterate(...)  │
                                         └────────────────┘
         △
         │ uses
         │
┌────────┴──────────────┐
│      Iteration        │
├───────────────────────┤
│ - strategy: Iiteration│
├───────────────────────┤
│ + iterate(List<Row>)  │
│ + setStrategy(...)    │
└───────────────────────┘
    STRATEGY PATTERN


┌─────────────────────────────────────────────────────────────────────────────┐
│                         VALIDATION PACKAGE                                  │
└─────────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────┐
│      <<interface>>               │
│        Validator                 │
├──────────────────────────────────┤
│ + validator()                    │
└──────────────────────────────────┘
         △
         │ implements
    ┌────┴────┬──────────────┬──────────────┬──────────────┐
    │         │              │              │              │
┌───┴────────────┐  ┌────────┴────────┐  ┌─┴──────────────┐  ┌──────────────┐
│TableExist      │  │TableMustExist   │  │SchemaMatch     │  │RowExist      │
│Validator       │  │Validator        │  │Validator       │  │Validator     │
├────────────────┤  ├─────────────────┤  ├────────────────┤  ├──────────────┤
│- dataBase      │  │- tableName      │  │- tableName     │  │- tableName   │
│- tableName     │  │- database       │  │- rows          │  │- rows        │
├────────────────┤  ├─────────────────┤  │- dataBase      │  │- database    │
│+ validator()   │  │+ validator()    │  ├────────────────┤  ├──────────────┤
└────────────────┘  └─────────────────┘  │+ validator()   │  │+ validator() │
                                         └────────────────┘  └──────────────┘

                    ┌─────────────────┐
                    │ConditionValid-  │
                    │ator             │
                    ├─────────────────┤
                    │- condition      │
                    │- schema         │
                    ├─────────────────┤
                    │+ validator()    │
                    └─────────────────┘
```

## 📊 Pattern Distribution

**Creational Patterns:** 3
- Singleton
- Builder
- Prototype

**Structural Patterns:** 1
- Composite

**Behavioral Patterns:** 3
- Command
- Observer
- Strategy

**Total:** 7 Design Patterns

---

## 📋 Design Patterns Summary List

### **1. Singleton Pattern** (Creational)
**Classes:**
- `Logger/Informer.java`
- `Logger/LogManager.java`
- `Logger/WriterLogger.java`

**Purpose:** Ensures single instance of logging components

**Key Features:**
- Thread-safe (synchronized)
- Lazy initialization
- Private constructor

---

### **2. Builder Pattern** (Creational)
**Classes:**
- `builder/Builder.java` (interface)
- `builder/TableBuilder.java`

**Purpose:** Constructs complex Table objects step-by-step

**Key Features:**
- Fluent API
- Separates construction from representation
- Multiple build methods

---

### **3. Prototype Pattern** (Creational)
**Classes:**
- `Clone/TableClone.java` (interface)
- `models/Table.java` (implements clone())

**Purpose:** Deep cloning of Table objects

**Key Features:**
- Copy constructor
- Deep copy of schema and rows
- Independent clones

---

### **4. Composite Pattern** (Structural)
**Classes:**
- `Condition/Condition.java` (interface)
- `Condition/SimpleCondition.java` (leaf)
- `Condition/AndCondition.java` (composite)
- `Condition/OrCondition.java` (composite)

**Purpose:** Hierarchical condition trees for queries

**Key Features:**
- Uniform treatment of simple and complex conditions
- Recursive structure
- Supports AND/OR logic

---

### **5. Command Pattern** (Behavioral)
**Classes:**
- `DataOperations/AbstractOperation.java` (abstract command)
- `DataOperations/InsertOperation.java`
- `DataOperations/UpdateOperation.java`
- `DataOperations/DeleteOperation.java`
- `DataOperations/QueryOperation.java`
- `DataOperations/CreateTable.java`
- `DataOperations/RemoveTable.java`

**Purpose:** Encapsulates operations as objects

**Key Features:**
- Uniform execution flow (validate → execute → return)
- Easy to extend
- Supports logging

---

### **6. Observer Pattern** (Behavioral)
**Classes:**
- `Logger/IObserver.java` (observer interface)
- `Logger/IInformer.java` (subject interface)
- `Logger/LogManager.java` (concrete observer)
- `Logger/Informer.java` (concrete subject)

**Purpose:** Reactive logging system

**Key Features:**
- Loose coupling
- Multiple observers support
- Automatic notifications

---

### **7. Strategy Pattern** (Behavioral)
**Classes:**
- `Iteration/Iiteration.java` (strategy interface)
- `Iteration/ByIncomeIteration.java` (concrete strategy)
- `Iteration/ByColumnIteration.java` (concrete strategy)
- `Iteration/ByConditionIteration.java` (concrete strategy)
- `Iteration/Iteration.java` (context)

**Purpose:** Interchangeable iteration algorithms

**Key Features:**
- Runtime algorithm selection
- Clean separation of concerns
- Easy to add new strategies

---
