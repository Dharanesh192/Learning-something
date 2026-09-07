# Understanding JSON File Structure

JSON (JavaScript Object Notation) is a lightweight, text-based data format used to store and exchange structured data. It's language-independent but easy for humans to read and for machines to parse.

## 1. Basic Syntax Rules

- Data is written as **key/value pairs**: `"key": value`
- Keys must always be strings, wrapped in double quotes
- Data is separated by commas
- Curly braces `{}` hold **objects**
- Square brackets `[]` hold **arrays**
- No trailing commas allowed
- No comments allowed in standard JSON

## 2. Data Types in JSON

| Type    | Example                  | Accessed By    |
|---------|---------------------------|---------------|
| String  | `"name": "Machi"`         | name    |
| Number  | `"age": 21`                | age    |
| Boolean | `"is_student": true`       | is_student    |
| Null    | `"middle_name": null`      | middle_name    |
| Object  | `"address": { "city": "Salem" }` | address\[city\]    |
| Array   | `"skills": ["Python", "Pandas"]` | skills\[0\]    |

## 3. Example JSON File

```json
{
  "name": "Dharanesh V",
  "age": 21,
  "is_student": true,
  "skills": ["Python", "Pandas", "NumPy", "Scikit-learn"],
  "college": {
    "name": "Velalar College of Engineering and Technology",
    "department": "AI and Data Science"
  },
  "projects": [
    {
      "title": "Focus Hub",
      "type": "Flutter Web PWA"
    },
    {
      "title": "Saily",
      "type": "AI Voice Assistant"
    }
  ],
  "graduation_year": null
}
```

## 4. Structure Breakdown

- **Root element**: The entire file is wrapped in one outer object `{ }` or array `[ ]`
- **Nested objects**: Objects can contain other objects (e.g., `"college"` above)
- **Arrays of objects**: A list of structured items (e.g., `"projects"`)
- **Scalar values**: Simple key/value pairs like strings, numbers, booleans

## 5. Common Use Cases

- Configuration files (`package.json`, `tsconfig.json`)
- API request/response payloads
- Data storage for NoSQL databases (e.g., MongoDB documents)
- Data interchange between frontend and backend
- Storing structured datasets for ML pipelines

## 6. Validation Tips

- Every opening brace/bracket needs a matching closing one
- Keys and string values must use **double quotes**, not single quotes
- Use a linter or online validator (e.g., jsonlint.com) before deploying
- In Python, use `json.load()` / `json.dumps()` to parse and generate JSON safely

## 7. Quick Python Example

```python
import json

# Reading a JSON file
with open("data.json", "r") as f:
    data = json.load(f)

# Writing a JSON file
with open("output.json", "w") as f:
    json.dump(data, f, indent=2)
```

## 8. Nested Key-Mapped Structure (Object → Object → Value)

A common pattern is a top-level object where each key maps to another object of platform/variant-specific values. This is the same shape whether you keep it as a Python dict or store it in a `.json` file.

```json
{
  "greetings": {
    "formal": {
      "english": "Good morning",
      "tamil": "Kaalai Vanakkam"
    },
    "casual": {
      "english": "Hey there",
      "tamil": "Vanakkam da"
    }
  },
  "app_status": {
    "online": {
      "label": "Connected",
      "color": "green"
    },
    "offline": {
      "label": "Disconnected",
      "color": "red"
    }
  }
}
```

Structurally, this is: **outer key → inner object → inner key → value**. It's a clean way to group related settings, translations, or mode-specific config under one label.

## 9. Accessing Nested Values

**In Python (dict, or JSON already loaded via `json.load`):**

```python
import json

with open("data.json", "r") as f:
    data = json.load(f)

# Direct access (raises KeyError if missing)
label = data["app_status"]["online"]["label"]

# Safe access with .get() — returns None instead of erroring
label = data.get("app_status", {}).get("online", {}).get("label")

# Pattern matching your example: outer key from a variable, inner key from another variable
category = "greetings"
mode = "casual"
lang = "tamil"
value = data[category][mode].get(lang)
print(value)  # Vanakkam da
```

**In JavaScript (after `JSON.parse`):**

```javascript
const data = JSON.parse(jsonString);

// Direct access
const label = data.app_status.online.label;

// Safe access with optional chaining
const label2 = data?.app_status?.online?.label;

// Dynamic keys from variables
const category = "greetings";
const mode = "casual";
const value = data[category]?.[mode]?.["tamil"];
```

**Key takeaway:** whether it's a Python dict or a parsed JSON object, nested lookups follow the same chain — `outer_key → inner_key → value`. Use `.get()` (Python) or `?.` optional chaining (JavaScript) when a key might not exist, to avoid crashes.

---
