# YAML — Complete Beginner-Friendly Learning Notes

> A beginner-friendly guide to understanding YAML from the basics to real-world usage.

---

## 📌 What is YAML?

**YAML** is a human-readable data serialization language commonly used for **configuration files, automation, CI/CD pipelines, container configuration, deployment settings, and structured data**.

YAML is designed to make structured information easy for humans to read and write.

For example, the following configuration:

```yaml
server:
  host: localhost
  port: 5000
  enabled: true
```

is easy to understand even without knowing a programming language.

Here:

- `server` is a key.
- `host`, `port`, and `enabled` are nested under `server`.
- `localhost` is a string.
- `5000` is a number.
- `true` is a boolean.
- Indentation defines the relationship between the values.

---

# 🧠 What does YAML stand for?

YAML is commonly expanded as:

> **YAML Ain't Markup Language**

Originally, YAML was associated with the phrase **"Yet Another Markup Language"**, but the name was later changed to emphasize that YAML is intended as a data serialization language rather than a markup language.

---

# 🤔 Why was YAML created?

When applications become larger, we often need to store settings separately from application code.

For example, an application might need:

```text
Server host
Server port
Database host
Database port
Application environment
Enabled features
Deployment settings
Build instructions
```

Instead of hard-coding all these settings into JavaScript, Python, Java, or another programming language, we can put them in a configuration file.

YAML provides a readable way to represent this information.

For example:

```yaml
application:
  name: SmartQuiz
  environment: development

server:
  host: localhost
  port: 5000

database:
  host: localhost
  port: 27017
```

The application or tool reads this YAML and uses the configuration.

---

# 🌍 Where is YAML used?

YAML appears in many parts of modern software development.

Common examples include:

- GitHub Actions
- GitLab CI/CD
- Docker Compose
- Kubernetes
- Ansible
- CI/CD pipelines
- Infrastructure configuration
- Deployment configuration
- Application configuration
- Automation tools
- Developer tooling

Some tools use YAML because it provides a convenient way to describe complex configuration in a human-readable form.

---

# 📁 `.yml` vs `.yaml`

You will commonly see two file extensions:

```text
config.yml
```

and:

```text
config.yaml
```

Both are commonly used for YAML files.

The YAML data format itself does not become a different format just because the extension changes.

For example:

```text
config.yml
```

and:

```text
config.yaml
```

can contain the same YAML syntax.

### Why are both used?

`.yml` became widely used historically, while `.yaml` is the full four-letter extension.

Both conventions became established in different projects and ecosystems.

Today, you will see both extensions in real-world projects.

### Which one should you use?

The safest rule is:

> **Follow the convention of the project or tool you are working with.**

If a tool's documentation specifically expects a particular filename or extension, follow that documentation.

Otherwise, choose one convention and remain consistent within your project.

For example:

```text
my-project/
├── config.yaml
├── database.yaml
└── deployment.yaml
```

or:

```text
my-project/
├── config.yml
├── database.yml
└── deployment.yml
```

Consistency is more important than constantly switching between the two.

---

# 🧱 Basic YAML Syntax

The most basic YAML structure is:

```yaml
key: value
```

Example:

```yaml
name: SmartQuiz
```

Here:

```text
key   → name
value → SmartQuiz
```

Another example:

```yaml
port: 5000
```

Here:

```text
key   → port
value → 5000
```

This simple `key: value` structure is one of the most important things to understand in YAML.

---

# 🏗️ Key-Value Pairs

YAML commonly represents information using key-value pairs.

```yaml
name: Dheerendra
role: Developer
language: JavaScript
```

We can think of it as:

```text
name     → Dheerendra
role     → Developer
language → JavaScript
```

Keys should clearly describe the information they contain.

For example:

```yaml
databaseHost: localhost
```

is more understandable than:

```yaml
x: localhost
```

when the configuration becomes larger.

---

# 📏 Indentation in YAML

One of the most important concepts in YAML is **indentation**.

YAML uses indentation to represent hierarchy.

Example:

```yaml
server:
  host: localhost
  port: 5000
```

Here:

```text
server
  ├── host
  └── port
```

`host` and `port` belong to `server`.

Now look at:

```yaml
application:
  server:
    host: localhost
    port: 5000
```

The structure becomes:

```text
application
  └── server
       ├── host
       └── port
```

The indentation tells the YAML parser which values belong to which parent.

---

# ⚠️ Spaces vs Tabs

This is one of the most common YAML mistakes.

### Use spaces

```yaml
server:
  host: localhost
  port: 5000
```

### Do not use tab characters for indentation

```yaml
server:
	host: localhost
	port: 5000
```

A tab character can cause a YAML parsing error.

A common convention is:

> **Use 2 spaces for each indentation level.**

For example:

```yaml
application:
  server:
    database:
      host: localhost
```

The exact indentation convention can vary, but the indentation must be consistent and valid for the structure being represented.

---

# 🔤 Strings

Strings represent text.

```yaml
name: SmartQuiz
environment: development
language: JavaScript
```

Strings can also be quoted.

### Double quotes

```yaml
name: "SmartQuiz"
```

### Single quotes

```yaml
name: "SmartQuiz"
```

For simple values, quotes are often unnecessary.

However, quotes can be useful when a value contains characters or formatting that could otherwise be interpreted specially.

---

# 🔢 Numbers

YAML can represent numeric values.

```yaml
port: 5000
age: 20
timeout: 30
price: 99.99
```

These values can be interpreted as numbers by YAML parsers.

If you intentionally need something to be treated as text, quoting it can make that intention clear.

For example:

```yaml
version: "1.0"
```

---

# ✅ Boolean Values

Boolean values represent true or false.

```yaml
enabled: true
debug: false
```

Example:

```yaml
server:
  enabled: true
  debug: false
```

A YAML parser can interpret these as boolean values rather than ordinary strings.

---

# ❌ Null Values

YAML can represent the absence of a value.

For example:

```yaml
database: null
```

You may also see:

```yaml
database:
```

depending on the context and parser.

Conceptually:

```text
database → no value
```

---

# 📋 Lists

YAML represents list items using a hyphen (`-`).

Example:

```yaml
languages:
  - JavaScript
  - Python
  - Java
  - C++
```

Conceptually:

```text
languages
├── JavaScript
├── Python
├── Java
└── C++
```

The hyphen indicates an item in the list.

---

# 🧑‍💻 List of Objects

Lists can contain objects.

For example:

```yaml
users:
  - name: Alice
    role: Admin

  - name: Bob
    role: Developer
```

Conceptually:

```text
users
├── User 1
│   ├── name
│   └── role
│
└── User 2
    ├── name
    └── role
```

This structure is extremely useful for representing collections of related objects.

---

# 🪆 Nested Objects

YAML supports nested structures.

Example:

```yaml
application:
  name: SmartQuiz

  server:
    host: localhost
    port: 5000

  database:
    host: localhost
    port: 27017
    name: smartquiz
```

The structure is:

```text
application
├── name
├── server
│   ├── host
│   └── port
│
└── database
    ├── host
    ├── port
    └── name
```

This is one of the main reasons YAML is useful for configuration.

---

# 📝 Comments

Comments start with `#`.

Example:

```yaml
# Server configuration

server:
  host: localhost
  port: 5000
```

The comment is not treated as configuration data.

You can also place a comment after a value:

```yaml
server:
  port: 5000 # Development server port
```

Comments are useful when a setting needs explanation.

---

# 🔐 Quoted Values

Sometimes values should be written inside quotes.

For example:

```yaml
version: "1.0"
```

or:

```yaml
message: "Hello: Welcome to YAML"
```

Quotes can help make it clear that something should be treated as a string, especially when the value resembles another YAML data type or contains syntax-sensitive characters.

---

# ⚠️ Special Characters

YAML uses some characters as part of its syntax.

Examples:

```text
:
-
#
[
]
{
}
|
>
```

For example:

```yaml
name: SmartQuiz
```

uses `:` to separate the key and value.

And:

```yaml
# comment
```

uses `#` to start a comment.

When a value contains special characters, quoting the value can sometimes make the intended string clearer.

Example:

```yaml
message: "Hello: Welcome to SmartQuiz"
```

---

# 📄 Multi-line Strings

YAML supports multi-line text.

One useful syntax is `|`.

```yaml
description: |
  YAML is a human-readable
  data serialization language.
  It is commonly used for configuration.
```

The `|` syntax preserves line breaks in the block.

Another syntax is `>`:

```yaml
description: >
  YAML is a human-readable
  data serialization language.
  It is commonly used for configuration.
```

The `>` syntax generally folds the lines into a single logical line.

These styles are useful when configuration contains longer text.

---

# 🧩 Flow Style

YAML also supports compact, inline structures.

Instead of:

```yaml
languages:
  - JavaScript
  - Python
  - Java
```

you can write:

```yaml
languages: [JavaScript, Python, Java]
```

Similarly, an object can be written as:

```yaml
server: { host: localhost, port: 5000 }
```

Compared with:

```yaml
server:
  host: localhost
  port: 5000
```

The multi-line style is often easier to read for larger configuration files.

---

# 🔗 Anchors and Aliases

YAML also provides features for reusing parts of configuration.

An **anchor** can define reusable data:

```yaml
defaults: &default_settings
  timeout: 30
  retries: 3
```

An **alias** can reference it:

```yaml
development:
  <<: *default_settings
  environment: development
```

Here:

```text
&default_settings
```

creates an anchor.

And:

```text
*default_settings
```

references that anchor.

This can reduce repetition in some configuration files.

However, because anchors and aliases can make YAML harder for beginners to read, they should be used only when they genuinely improve the configuration.

---

# 🔄 YAML Can Represent Structured Data

YAML can represent:

- strings
- numbers
- booleans
- null
- lists
- mappings/objects
- nested structures

For example:

```yaml
application:
  name: SmartQuiz
  version: "1.0"
  enabled: true

  server:
    host: localhost
    port: 5000

  languages:
    - JavaScript
    - Python

  database: null
```

This one example contains several YAML concepts together.

---

# 🌎 Real-World Example

A more realistic configuration could look like:

```yaml
application:
  name: SmartQuiz
  environment: development

server:
  host: localhost
  port: 5000
  enabled: true

database:
  type: mongodb
  host: localhost
  port: 27017
  name: smartquiz

features:
  authentication: true
  analytics: true
  aiQuizGeneration: true

supportedLanguages:
  - English
  - Hindi

admins:
  - name: Alice
    role: Admin

  - name: Bob
    role: Developer
```

Notice how the indentation makes the relationships visible.

---

# ⚙️ YAML in GitHub Actions

GitHub Actions uses YAML files to define workflows.

A simplified example:

```yaml
name: Node.js CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

Understanding YAML makes this easier to read.

Conceptually:

```text
workflow
├── name
├── trigger
│   └── push
│       └── branches
│
└── jobs
    └── build
        ├── runs-on
        └── steps
```

The YAML provides the structure.

GitHub Actions decides what that structure means and how to execute it.

---

# 🐳 YAML in Docker Compose

Docker Compose commonly uses YAML to describe services.

Example:

```yaml
services:
  backend:
    build: .
    ports:
      - "5000:5000"

  database:
    image: mongo
    ports:
      - "27017:27017"
```

Here:

```text
services
├── backend
│   ├── build
│   └── ports
│
└── database
    ├── image
    └── ports
```

Again, YAML is describing the configuration.

Docker Compose interprets that configuration.

---

# ☸️ YAML in Kubernetes

Kubernetes commonly uses YAML manifests to describe resources.

Example:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: backend

spec:
  replicas: 2

  selector:
    matchLabels:
      app: backend

  template:
    metadata:
      labels:
        app: backend

    spec:
      containers:
        - name: backend
          image: my-backend:latest
          ports:
            - containerPort: 5000
```

This may look complicated initially, but the same YAML concepts are still present:

- key-value pairs
- nested objects
- lists
- indentation

The difficult part here is usually not YAML itself.

The difficult part is understanding **Kubernetes**.

That distinction is important.

---

# 🆚 YAML vs JSON

YAML and JSON can both represent structured data.

### JSON

```json
{
  "server": {
    "host": "localhost",
    "port": 5000,
    "enabled": true
  }
}
```

### YAML

```yaml
server:
  host: localhost
  port: 5000
  enabled: true
```

Both represent similar information.

### YAML advantages

- Human-readable
- Less punctuation
- Indentation makes hierarchy visible
- Comments are supported
- Common in configuration and automation

### JSON advantages

- Very common for APIs
- Strict and predictable syntax
- Widely supported by programming languages
- Easy for machines to generate and consume

It is not really a question of:

> "Which one is better?"

The better format depends on the problem and the tool you are using.

---

# 🧠 YAML vs Programming Languages

YAML is not normally used to implement application logic.

For example:

```yaml
port: 5000
```

does not start a server.

Instead, an application can read this configuration and use the value.

Think of it like:

```text
YAML
  ↓
Configuration
  ↓
Application / Tool
  ↓
Actual behavior
```

For example:

```yaml
server:
  port: 5000
```

could be read by a Node.js application.

The Node.js application is responsible for actually starting the server.

---

# ❌ Common YAML Mistakes

## 1. Using tabs

Incorrect:

```yaml
server:
	host: localhost
```

Correct:

```yaml
server:
  host: localhost
```

Use spaces for indentation.

---

## 2. Incorrect indentation

Incorrect:

```yaml
server:
  host: localhost
    port: 5000
```

Correct:

```yaml
server:
  host: localhost
  port: 5000
```

Both children need to be correctly aligned.

---

## 3. Missing colon

Incorrect:

```yaml
server
  port: 5000
```

Correct:

```yaml
server:
  port: 5000
```

---

## 4. Incorrect list indentation

Incorrect:

```yaml
languages:
  - JavaScript
    - Python
```

Correct:

```yaml
languages:
  - JavaScript
  - Python
```

---

## 5. Mixing indentation levels

Incorrect:

```yaml
server:
  host: localhost
   port: 5000
```

The indentation is inconsistent.

Correct:

```yaml
server:
  host: localhost
  port: 5000
```

---

## 6. Assuming every value is a string

YAML can interpret values according to its data type rules.

For example:

```yaml
enabled: true
```

is intended as a boolean.

While:

```yaml
enabled: "true"
```

is a string.

When the exact type matters, be explicit.

---

# ⚠️ YAML Type and Parsing Gotchas

YAML is convenient partly because it can infer types.

But implicit type interpretation can sometimes surprise beginners.

For example:

```yaml
enabled: true
```

and:

```yaml
enabled: "true"
```

do not necessarily represent the same type.

The first is a boolean.

The second is a string.

Similarly:

```yaml
version: "1.0"
```

makes it clear that the version is intended as text.

The exact behavior of some YAML values can also depend on the YAML specification/version and parser being used.

So when a configuration value has important type requirements, check the documentation of the tool and parser you are using.

---

# 🔍 Validating YAML

A YAML file can look visually correct and still contain a syntax error.

For example:

```yaml
server:
  host: localhost
   port: 5000
```

A YAML parser can detect the problem.

Useful ways to validate YAML include:

- editor/IDE YAML extensions
- YAML linters
- the parser provided by your programming language
- the tool that consumes the YAML
- CI checks

Validation is especially important for deployment and CI/CD configuration.

A small indentation mistake can prevent an entire workflow or deployment from working.

---

# 🛠️ YAML Best Practices

## 1. Use consistent indentation

A common convention is:

```text
2 spaces
```

Example:

```yaml
server:
  host: localhost
  port: 5000
```

---

## 2. Never use tabs for indentation

Use spaces.

---

## 3. Keep configuration readable

Prefer:

```yaml
server:
  host: localhost
  port: 5000
```

over unnecessarily compressed configuration when readability suffers.

---

## 4. Use meaningful names

Prefer:

```yaml
database:
  host: localhost
```

over:

```yaml
db:
  x: localhost
```

when the longer names make the configuration easier to understand.

---

## 5. Use comments where they help

```yaml
server:
  port: 5000 # Local development port
```

Don't add comments just to explain things that are already obvious.

---

## 6. Avoid unnecessary complexity

YAML supports advanced features such as anchors and aliases.

Use them when they make configuration better, not simply because they exist.

---

## 7. Follow the tool's documentation

A YAML file is ultimately interpreted by another tool.

For example:

```text
GitHub Actions → GitHub Actions syntax
Docker Compose → Compose specification
Kubernetes → Kubernetes API/resource schema
```

Knowing YAML syntax alone does not mean you automatically know the configuration rules of these tools.

---

# 🧩 YAML Has Two Different Things to Understand

This distinction helped me understand YAML better.

### Part 1 — YAML syntax

For example:

```yaml
server:
  port: 5000
```

This is about how YAML represents data.

### Part 2 — Tool-specific configuration

For example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

The YAML syntax is one thing.

What `jobs`, `build`, and `runs-on` mean is defined by **GitHub Actions**.

Similarly:

```yaml
services:
  backend:
```

is YAML syntax, but what `services` means depends on Docker Compose.

So:

> **Learning YAML syntax is not the same as learning GitHub Actions, Docker Compose, or Kubernetes.**

YAML is the language/format used to describe the configuration; the consuming tool defines the meaning of the configuration.

---

# 📚 A Complete YAML Example

Here is one example combining many concepts:

```yaml
# Application configuration

application:
  name: SmartQuiz
  version: "1.0"
  environment: development
  enabled: true

server:
  host: localhost
  port: 5000

database:
  type: mongodb
  host: localhost
  port: 27017
  name: smartquiz

features:
  authentication: true
  analytics: true
  aiQuizGeneration: true

languages:
  - English
  - Hindi

users:
  - name: Alice
    role: Admin

  - name: Bob
    role: Developer

description: |
  SmartQuiz is a learning platform.
  This configuration is written in YAML.
```

This example demonstrates:

- comments
- strings
- quoted strings
- numbers
- booleans
- nested objects
- lists
- lists of objects
- multi-line strings
- indentation

---

# 📝 YAML Quick Cheat Sheet

## Key-Value

```yaml
name: SmartQuiz
```

## Nested Object

```yaml
server:
  host: localhost
  port: 5000
```

## List

```yaml
languages:
  - JavaScript
  - Python
```

## List of Objects

```yaml
users:
  - name: Alice
    role: Admin

  - name: Bob
    role: Developer
```

## Boolean

```yaml
enabled: true
```

## Null

```yaml
database: null
```

## Comment

```yaml
# This is a comment
```

## Multi-line Text

```yaml
description: |
  This is
  multiple lines
  of text.
```

## Inline List

```yaml
languages: [JavaScript, Python, Java]
```

## Inline Object

```yaml
server: { host: localhost, port: 5000 }
```

## Anchor

```yaml
defaults: &defaults
  timeout: 30
```

## Alias

```yaml
development:
  <<: *defaults
```

---

# 🎯 What I Learned From YAML

The biggest thing I learned is that YAML is not difficult because it has a huge amount of syntax.

Its core ideas are actually quite small:

```text
key: value
```

for key-value pairs,

```yaml
items:
  - item1
  - item2
```

for lists,

and indentation:

```yaml
parent:
  child:
    value: something
```

for hierarchy.

The tricky part is being consistent and understanding how the parser interprets the structure.

---

# 💡 My Mental Model for Reading YAML

Whenever I open a YAML file, I try to understand it using these questions:

### 1. What is the key?

```yaml
server:
```

### 2. What is the value?

```yaml
port: 5000
```

### 3. Is this a list?

```yaml
languages:
  - JavaScript
  - Python
```

### 4. What belongs to what?

Look at the indentation:

```yaml
application:
  server:
    port: 5000
```

Read it as:

```text
application
    ↓
server
    ↓
port = 5000
```

This simple mental model makes large YAML files much easier to understand.

---

# 🚀 What to Learn After YAML

If you're learning YAML as part of becoming a better developer, a useful progression can be:

```text
YAML Basics
     ↓
Git & GitHub
     ↓
GitHub Actions
     ↓
Docker
     ↓
Docker Compose
     ↓
CI/CD
     ↓
Cloud / Deployment
     ↓
Kubernetes
```

You don't need to learn everything at once.

First understand YAML itself.

Then understand how individual tools use it.

---

# 📌 Final Takeaway

YAML may look like a simple configuration format, but it is worth understanding properly because it appears in many areas of modern software development.

The most important things to remember are:

```text
1. YAML represents structured data.
2. YAML is designed to be human-readable.
3. Indentation defines hierarchy.
4. Use spaces, not tabs, for indentation.
5. Lists use `-`.
6. Key-value pairs use `key: value`.
7. YAML supports strings, numbers, booleans, null, lists, and nested structures.
8. `.yml` and `.yaml` are both commonly used YAML extensions.
9. Follow the conventions/documentation of the tool you're using.
10. YAML syntax and tool-specific configuration are two different things.
11. Validate YAML before relying on it for important automation or deployment.
12. Readability and consistency matter.
```

---

## 🌱 Why I Am Documenting This

I'm learning different concepts as I continue my development journey, and I don't want my learning to remain limited to temporary notes.

So I'm documenting concepts in a way that is useful for:

- **me** — so I can come back and revise them later
- **other beginners** — so they can understand the concept from a learner's perspective
- **future projects** — so I have practical examples to refer back to

This repository is not meant to be an official specification or replacement for documentation.

It's my **learning journey, notes, explanations, mistakes, examples, and understanding** of the technologies I'm learning.

> **Learn → Understand → Build → Document → Share**
