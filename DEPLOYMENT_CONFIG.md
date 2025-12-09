# WashTrack Deployment Configuration

## Status: ✅ Environment Variables Configured

### Changes Made (December 9, 2025)

#### 1. **Created `.env` File**

- Location: `WashTrack/.env`
- Configuration:
  ```
  VITE_API_URL=https://backend-production-e6b1.up.railway.app
  ```
- Purpose: Central configuration for Railway backend URL

#### 2. **Updated All API Endpoints**

- Total endpoints replaced: **31 occurrences** across 9 files
- Pattern: Replaced `'http://localhost:8081'` with `` `${import.meta.env.VITE_API_URL}` ``
- Files updated:
  1.  **AdminOrders.jsx** (9 occurrences)
      - fetchOrders, fetchServices, updateStatus, createReport, editOrder, deleteOrder, getUser, createOrder
  2.  **Orders.jsx** (2 occurrences)
      - refreshOrderData, fetchUserOrders
  3.  **Tracking.jsx** (2 occurrences)
      - refreshOrderData, fetchUserOrders
  4.  **Services.jsx** (4 occurrences)
      - fetchServices, deleteService, editService, createService
  5.  **Reports.jsx** (3 occurrences)
      - fetchReports, fetchOrders, fetchAllUsers
  6.  **Register.jsx** (1 occurrence)
      - signupuser endpoint
  7.  **Profile.jsx** (5 occurrences)
      - getUser, getProfile, verifyPassword, updatePassword, updateUser, updateProfile
  8.  **Management.jsx** (5 occurrences)
      - fetchStaff, fetchServices, deleteStaff, editStaff, createStaff
  9.  **Login.jsx** (2 occurrences)
      - loginadmin, loginuser

### Technical Details

#### Environment Variable Integration

- **Variable Name**: `VITE_API_URL`
- **Prefix**: `VITE_` (Vite requires this prefix for client-side exposure)
- **Value**: `https://backend-production-e6b1.up.railway.app`
- **Usage Pattern**:

  ```javascript
  // Before
  fetch("http://localhost:8081/orders");

  // After
  fetch(`${import.meta.env.VITE_API_URL}/orders`);
  ```

#### Deployment Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Netlify Frontend                          │
│  (WashTrack React App - deployed on Netlify)               │
│                                                              │
│  Uses: import.meta.env.VITE_API_URL                        │
│  Points to: https://backend-production-e6b1.up.railway.app │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               │ HTTPS API Calls
                               │
┌──────────────────────────────▼──────────────────────────────┐
│                   Railway Backend                            │
│  (Node.js Express API - deployed on Railway)               │
│  URL: https://backend-production-e6b1.up.railway.app       │
│                                                              │
│  Database: MySQL (washtrack_db on Railway)                 │
└─────────────────────────────────────────────────────────────┘
```

### Verification Checklist

#### ✅ Configuration Complete

- [x] `.env` file created with correct Railway backend URL
- [x] All 31 localhost:8081 references replaced with environment variable
- [x] No remaining hardcoded localhost URLs in WashTrack components
- [x] Environment variable uses proper Vite syntax (`VITE_` prefix)

#### ⏳ Next Steps (Required for Deployment)

1. **CORS Configuration** (May be needed)

   - If CORS errors appear in browser console, add Netlify domain to server.js:
     ```javascript
     app.use(
       cors({
         origin: "https://your-netlify-domain.netlify.app",
         credentials: true,
       })
     );
     ```
   - Find your Netlify domain in Netlify Dashboard → Settings → Site information

2. **Test Connectivity**

   - Deploy to Netlify (if not already done)
   - Open Netlify-deployed site in browser
   - Open browser DevTools → Network tab
   - Try logging in or viewing orders
   - Verify API calls go to `backend-production-e6b1.up.railway.app`
   - Check for any CORS or 404 errors

3. **Environment Variable in Netlify**
   - Option A: Keep using `.env` file (Vite automatically reads it)
   - Option B: Set in Netlify Dashboard → Build & Deploy → Environment
     - Key: `VITE_API_URL`
     - Value: `https://backend-production-e6b1.up.railway.app`

### Local Development

For local development with localhost backend:

```bash
# Create .env.local file
VITE_API_URL=http://localhost:8081

# Vite loads .env.local and overrides .env during local development
```

### Troubleshooting

#### Issue: API calls still hitting localhost

- Check if `.env` file is in project root (WashTrack folder)
- Vite needs to restart to pick up new `.env` changes
- Run: `npm run dev` or `vite` to reload

#### Issue: CORS errors in browser console

- Origin error: Backend CORS not configured for Netlify domain
- Solution: Update `server.js` CORS settings with Netlify domain

#### Issue: 404 on API calls

- Verify Railway backend is running and accessible
- Check Railway dashboard for service status
- Verify endpoint URLs match backend routes

### Files Modified

- `WashTrack/.env` (created)
- `WashTrack/src/components/AdminOrders.jsx`
- `WashTrack/src/components/Orders.jsx`
- `WashTrack/src/components/Tracking.jsx`
- `WashTrack/src/components/Services.jsx`
- `WashTrack/src/components/Reports.jsx`
- `WashTrack/src/components/Register.jsx`
- `WashTrack/src/components/Profile.jsx`
- `WashTrack/src/components/Management.jsx`
- `WashTrack/src/components/Login.jsx`

### Deployment Status

- **Frontend**: Ready for Netlify deployment ✅
- **Backend**: Deployed on Railway ✅
- **Environment Configuration**: Complete ✅
- **CORS Configuration**: Pending (may not be needed) ⏳
- **Testing**: Awaiting deployment test ⏳

---

## Reference: Environment Variable Locations

### API Endpoints Summary

| Component   | Endpoints        | Status             |
| ----------- | ---------------- | ------------------ |
| AdminOrders | 9 endpoints      | ✅ Updated         |
| Login       | 2 endpoints      | ✅ Updated         |
| Orders      | 2 endpoints      | ✅ Updated         |
| Tracking    | 2 endpoints      | ✅ Updated         |
| Services    | 4 endpoints      | ✅ Updated         |
| Reports     | 3 endpoints      | ✅ Updated         |
| Register    | 1 endpoint       | ✅ Updated         |
| Profile     | 5 endpoints      | ✅ Updated         |
| Management  | 5 endpoints      | ✅ Updated         |
| **TOTAL**   | **31 endpoints** | **✅ All Updated** |
