# 📬 QA76m_Phonebook_Postman

API test automation for the **Phonebook REST API** using **Postman**, **Newman**, and **GitHub Actions CI/CD**.

> Data-driven API testing with automated execution on every push.

---

## 🎯 What's Covered

| Endpoint | Tests |
|---|---|
| `POST /v1/user/login/usernamepassword` | Login (positive + negative scenarios) |
| `GET /v1/contacts` | Get all contacts (authenticated) |
| `POST /v1/contacts` | Add contact (positive + validation errors) |
| `DELETE /v1/contacts/{id}` | Delete contact by ID |

Tests include status code checks, response body validation, JSON schema assertions, and chained requests (login → use token → call protected endpoint).

---

## 🧰 Tech Stack

- **Postman** — collection design, test scripting (`pm.test`, `pm.expect`)
- **Postman CLI** — CI execution
- **GitHub Actions** — CI/CD trigger on push
- **JavaScript** — Postman test scripts (Chai-style assertions)

---

## 🏗️ Project Structure

```
.
├── Postman Collections/
│   └── collection.json.json        # Exported Postman collection with all tests
├── .github/
│   └── workflows/
│       └── main.yml                 # GitHub Actions CI pipeline
└── README.md
```

---

## 🔄 CI/CD Pipeline

The workflow (`.github/workflows/main.yml`) runs on **every push**:

1. Checks out the repository
2. Installs Postman CLI
3. Authenticates with Postman API key (stored in GitHub Secrets)
4. Executes the collection with the linked environment

```yaml
on: push

jobs:
  automated-api-tests:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Postman CLI
        run: powershell.exe ...
      - name: Login to Postman CLI
        run: postman login --with-api-key ${{ secrets.POSTMAN_API_KEY }}
      - name: Run API tests
        run: postman collection run "{collection_id}" -e "{environment_id}"
```

### Setup for your fork
1. Add `POSTMAN_API_KEY` to **Settings → Secrets and variables → Actions**
2. Replace collection/environment IDs in `main.yml` with your own
3. Push — workflow runs automatically

---

## 🔑 Key Patterns

- **Data-driven tests** — Postman iterations with CSV/JSON data files
- **Token chaining** — login response → save JWT to environment variable → reuse in subsequent requests
- **Negative scenarios** — wrong credentials, missing fields, invalid IDs
- **Schema validation** — `pm.response.to.have.jsonSchema(...)` for response contract checks

---

## 🚀 How to Run Locally

### Option 1: Postman GUI
1. Import `Postman Collections/collection.json.json` into Postman
2. Set up the environment (baseUrl, credentials)
3. Click **Run collection**

### Option 2: Newman CLI
```bash
npm install -g newman
newman run "Postman Collections/collection.json.json" \
       -e environment.json \
       --reporters cli,html \
       --reporter-html-export report.html
```

---

## 📝 Notes

- Backend: **Telran Phonebook training API** (Heroku-hosted Spring Boot).
- The CI workflow uses `windows-latest` — for faster/cheaper builds, switching to `ubuntu-latest` is recommended in a future iteration.

---

## 🎓 Author

**Serdar Kerimov** — [github.com/xscofild](https://github.com/xscofild) · [LinkedIn](https://www.linkedin.com/in/serdarkerimov/)
QA Engineer | Java · Selenium · REST Assured · SQL
