# Developers
Ali Najar 401102701

Amirreza Inanloo 401105667

# SimpleNote — iOS (SwiftUI)

SwiftUI client for the SimpleNote backend: onboarding → auth (register/login) → notes (search + pagination) → editor (create/edit/delete) → settings (profile, change password, logout). Stores JWT tokens in Keychain with **auto-refresh** on 401.

---

## Prerequisites
- Xcode 15+
- iOS 16+ target
- Backend running locally or accessible over the network
  - Redoc & Postman import: `/api/schema/redoc/`

---

## Features
- Onboarding with centered illustration
- Register / Login
- Keychain token storage (access + refresh), auto-refresh on 401, retry original request
- Notes list with search (`/api/notes/filter`) and pagination
- Create/Update/Delete notes
- Settings: profile (`/api/auth/userinfo/`), change password, logout
- Friendly error messages from backend responses

---

## API Endpoints (expected)
- `POST /api/auth/register/` → `{ username, password, email, first_name?, last_name? }`
- `POST /api/auth/token/` → `{ username, password }` ⇒ `{ access, refresh }`
- `POST /api/auth/token/refresh/` → `{ refresh }` ⇒ `{ access }`
- `GET  /api/auth/userinfo/`
- `POST /api/auth/change-password/` → `{ old_password, new_password }`

Notes:
- `GET  /api/notes/?page=&page_size=`
- `GET  /api/notes/filter?title=&description=&page=&page_size=`
- `GET  /api/notes/{id}/`
- `POST /api/notes/` → `{ title, description }`
- `PUT  /api/notes/{id}/` → `{ title, description }`
- `DELETE /api/notes/{id}/`

---

## Project Layout (high level)
```
Config.swift          # Base URL
APIClient.swift       # URLSession client, auto-refresh, error handling
Keychain.swift        # Secure token storage
DTOs.swift            # Codable models
ViewModels.swift      # Login, Register, Home, Editor, ChangePassword
Views.swift           # Onboarding, Login, Register, Home, Editor, Settings, ChangePassword
App.swift             # Entry point, RootView
Assets.xcassets/      # Images (add onboarding_illustration)
```
