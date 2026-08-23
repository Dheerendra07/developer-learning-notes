# JSON, YAML & XML — Learning Repository

A structured learning repository for understanding **JSON, YAML/YML, and XML** from the basics through practical examples.

The repository is organized so that each topic has its own detailed `README.md`, while this main README provides the overall learning path and structure.

---

## 📚 Topics Covered

| Topic          | What You Will Learn                                                             | Detailed Notes                  |
| -------------- | ------------------------------------------------------------------------------- | ------------------------------- |
| **JSON**       | Syntax, objects, arrays, data types, nesting, and practical data representation | [Open JSON →](./JSON/README.md) |
| **YAML / YML** | Indentation, mappings, sequences, nested data, and configuration-style files    | [Open YAML →](./YAML/README.md) |
| **XML**        | Elements, attributes, nesting, XML structure, and data representation           | [Open XML →](./XML/README.md)   |

---

## 🧭 Learning Path

```text
                    JSON • YAML • XML
                           │
                           ▼
                    Basic Structure
                           │
            ┌──────────────┼──────────────┐
            ▼              ▼              ▼
          JSON            YAML            XML
            │              │              │
            ▼              ▼              ▼
         Syntax         Syntax         Syntax
            │              │              │
            ▼              ▼              ▼
      Objects/Arrays   Mappings/Lists  Elements/
                                      Attributes
            │              │              │
            └──────────────┼──────────────┘
                           ▼
                    Nested Data
                           │
                           ▼
                 Practical Examples
                           │
                           ▼
                  Compare & Understand
```

The goal is not just to memorize syntax, but to understand **how these formats represent structured data and where their structures differ**.

---

## 🗂️ Repository Structure

```text
Learning-Repository/
│
├── README.md
│
├── JSON/
│   └── README.md
│
├── YAML/
│   └── README.md
│
└── XML/
    └── README.md
```

### How to use this structure

```text
README.md
   │
   ├── Start here
   │
   ├── JSON/README.md
   │      └── Learn JSON in detail
   │
   ├── YAML/README.md
   │      └── Learn YAML in detail
   │
   └── XML/README.md
          └── Learn XML in detail
```

---

## 🔍 Quick Understanding

### JSON

**JSON (JavaScript Object Notation)** is commonly used to represent and exchange structured data.

Example:

```json
{
  "name": "Dheerendra",
  "age": 20,
  "skills": ["JavaScript", "React"]
}
```

**Think:** data represented using **objects and arrays**.

→ [Learn JSON in detail](./JSON/README.md)

---

### YAML / YML

**YAML** is a human-readable format commonly used for configuration and structured data.

Example:

```yaml
name: Dheerendra
age: 20
skills:
  - JavaScript
  - React
```

**Think:** structured data represented mainly through **indentation**.

→ [Learn YAML in detail](./YAML/README.md)

---

### XML

**XML (eXtensible Markup Language)** represents data using **tags and nested elements**.

Example:

```xml
<student>
  <name>Dheerendra</name>
  <age>20</age>
  <skill>JavaScript</skill>
</student>
```

**Think:** structured data represented through **tags and elements**.

→ [Learn XML in detail](./XML/README.md)

---

## 🆚 One-View Comparison

```text
JSON
│
├── Uses { } and [ ]
├── Key-value based
└── Common in APIs and data exchange


YAML
│
├── Uses indentation
├── Minimal syntax
└── Common in configuration files


XML
│
├── Uses opening/closing tags
├── Supports attributes
└── Common in structured documents and some legacy systems
```

The three formats can represent similar information, but their **syntax, readability, and common use cases differ**.

---

## 🎯 Learning Approach

Each topic is studied separately using the same general pattern:

```text
Concept
   ↓
Basic Syntax
   ↓
Data Types / Structures
   ↓
Nested Data
   ↓
Examples
   ↓
Practical Understanding
   ↓
Comparison
```

This makes it easier to understand not only **what the syntax looks like**, but also **how the data is structured**.

---

## 📌 Current Progress

```text
JSON       ──────────────── ✓
YAML / YML ──────────────── ✓
XML        ──────────────── ✓
```

More structured-data and configuration formats can be added to this repository later without changing the existing learning structure.

---

## 🚀 Where to Start

If you are new to these formats:

```text
1. JSON
   ↓
2. YAML / YML
   ↓
3. XML
   ↓
4. Compare their structures
```

Start with **[JSON](./JSON/README.md)** and then move through YAML and XML.

---

## 📝 Note

This repository focuses on **understanding the structure and syntax from scratch**, with examples kept close to the concepts being learned.

The individual topic `README.md` files contain the detailed explanations and examples.
