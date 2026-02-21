# Vercel Deployment Guide

## Quick Deployment Steps

### Step 1: Prepare Your Backend API
Your frontend expects a backend API. You need to:
- Deploy your backend server first (e.g., on Render, Railway, Heroku, or AWS)
- Note the production URL (e.g., `https://your-backend.onrender.com`)

### Step 2: Update Environment Variables
1. Open `.env.production` file
2. Replace `https://your-backend-domain.com/api` with your actual backend URL:
   ```
   VITE_API_BASE_URL=https://your-backend.onrender.com/api
   ```

### Step 3: Install Vercel CLI (Optional but Recommended)
```bash
npm i -g vercel
```

### Step 4: Deploy to Vercel

#### Option A: Deploy via Vercel CLI
1. Login to Vercel:
   ```bash
   vercel login
   ```

2. Deploy:
   ```bash
   vercel --prod
   ```

3. Set environment variables on Vercel:
   ```bash
   vercel env add VITE_API_BASE_URL
   # Enter your backend URL when prompted
   ```

#### Option B: Deploy via Git (Recommended)
1. Push your code to GitHub/GitLab/Bitbucket
2. Go to [vercel.com](https://vercel.com) and sign up/login
3. Click "Add New Project"
4. Import your repository
5. Configure build settings:
   - **Framework Preset**: Vite
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
6. Add Environment Variables:
   - `VITE_API_BASE_URL` = `https://your-backend.onrender.com/api`
7. Click "Deploy"

### Step 5: Configure Backend CORS (Important!)
Your backend must accept requests from your Vercel domain. Add this to your backend:

```javascript
// Node.js/Express example
app.use(cors({
  origin: ['https://your-vercel-app.vercel.app', 'http://localhost:5173'],
  credentials: true
}));
```

## File Structure Added

```
Frontend/
├── vercel.json          # Vercel routing configuration
├── .env.production      # Production environment variables
└── DEPLOYMENT.md        # This file
```

## Important Notes

1. **Client-Side Routing**: The `vercel.json` file ensures React Router works correctly by rewriting all routes to `index.html`

2. **API Calls**: All API calls must use `import.meta.env.VITE_API_BASE_URL` instead of `/api` or localhost URLs

3. **Environment Variables**: Vite uses `VITE_` prefix for env vars. These are embedded at build time.

4. **Backend Required**: This is a frontend-only deployment. You still need a separate backend deployment.

## Troubleshooting

### 404 Errors on Page Refresh
→ `vercel.json` should handle this. Make sure it's committed and pushed.

### API Calls Not Working
→ Check that `VITE_API_BASE_URL` is set correctly in Vercel dashboard under Project Settings > Environment Variables

### CORS Errors
→ Configure CORS on your backend to allow your Vercel domain

### Blank Page After Deploy
→ Check browser console for errors. Common causes:
- Missing environment variables
- Build errors (check Vercel build logs)

## Post-Deployment Checklist

- [ ] Backend API is deployed and accessible
- [ ] `VITE_API_BASE_URL` updated in `.env.production`
- [ ] Environment variables set in Vercel dashboard
- [ ] CORS configured on backend
- [ ] All pages load correctly (test navigation)
- [ ] API calls work (test login or data fetching)
- [ ] Images and assets load correctly

## Need Help?

- Vercel Docs: https://vercel.com/docs
- Vite Deployment: https://vitejs.dev/guide/static-deploy.html#vercel
