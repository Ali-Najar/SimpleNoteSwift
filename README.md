# SimpleNote — iOS (SwiftUI)

SwiftUI client for the SimpleNote backend: onboarding → auth (register/login) → notes (search + pagination) → editor (create/edit/delete) → settings (profile, change password, logout). Stores JWT tokens in Keychain with **auto-refresh** on 401.

---

## Prerequisites
- Xcode 15+
- iOS 16+ target
- Backend running locally or accessible over the network
  - Redoc & Postman import: `/api/schema/redoc/`

---

## Quick Start

1) **Run backend** (Docker example):
```bash
docker compose up --build
# API docs: http://localhost:8000/api/schema/redoc/
```

2) **Set API base URL** in `Config.swift`:
```swift
enum Config {
    static let API_BASE_URL = URL(string: "http://127.0.0.1:8000/")! // Simulator + backend on your Mac
}
```
> For a real device, use your Mac’s LAN IP (e.g., `http://192.168.1.10:8000/`).

3) **Allow HTTP during development** (ATS exception if not using HTTPS). Add to **Info.plist**:
```xml
<key>NSAppTransportSecurity</key>
<dict>
  <key>NSAllowsArbitraryLoads</key>
  <true/>
</dict>
```
> Prefer HTTPS in production and remove this exception.

4) **Build & Run** in the simulator.

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

---

## Troubleshooting

**Simulator can’t reach backend**  
- Use `http://127.0.0.1:8000/` for backend on your Mac.  
- Add ATS exception for HTTP.  
- If Docker is on another host, use that machine’s IP.

**401/expired token**  
- Client refreshes access token automatically using stored refresh token; check backend `/api/auth/token/refresh/`.

**Registration 400**  
- Backend validation (weak password or duplicate username/email). The UI surfaces the server message.

**Delete 404**  
- Wrong note id or unauthenticated request (refresh failed). Check the response body in the UI.

---

## License
MIT (or your preferred license).
