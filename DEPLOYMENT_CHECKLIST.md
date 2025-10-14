# 🚀 LaserOstop España - Deployment Checklist

## ✅ Pre-Deployment Complete

- [x] Environment variables configured (.env created)
- [x] Backend updated to use environment variables
- [x] Frontend API URL supports production
- [x] .gitignore files created
- [x] Netlify configuration (netlify.toml)
- [x] Deployment documentation created
- [x] CORS configured for production

---

## 📋 Deployment Steps

### 1️⃣ Backend Deployment (Choose One)

#### Option A: Render.com (Recommended - Free)
- [ ] Create account at https://render.com
- [ ] Create new Web Service
- [ ] Connect GitHub repository
- [ ] Configure:
  - Root Directory: `lp_code/backend`
  - Build: `npm install`
  - Start: `npm start`
- [ ] Add ALL environment variables from `.env` file:
  ```
  SMART_AGENDA_BASE_URL
  SMART_AGENDA_LOGIN
  SMART_AGENDA_PWD
  SMART_AGENDA_API_ID
  SMART_AGENDA_API_KEY
  NODE_ENV=production
  PORT=3000
  CORS_ORIGIN=*
  ```
- [ ] Deploy and get URL: `https://your-app.onrender.com`
- [ ] Test: `curl https://your-app.onrender.com/api/health`

#### Option B: Railway.app
- [ ] Create account at https://railway.app
- [ ] Follow similar steps as Render
- [ ] Add environment variables
- [ ] Get URL and test

---

### 2️⃣ Frontend Deployment (Netlify)

- [ ] Create account at https://netlify.com
- [ ] Click "Add new site" → "Import from Git"
- [ ] Connect GitHub repository
- [ ] Configure:
  - Base directory: `lp_code`
  - Build command: (leave empty)
  - Publish directory: `.`
- [ ] Deploy site
- [ ] Get Netlify URL: `https://your-site.netlify.app`
- [ ] **IMPORTANT**: Update `index.html` line 551 with your actual backend URL
- [ ] Push changes to trigger redeploy

---

### 3️⃣ Connect Frontend to Backend

- [ ] Open `lp_code/index.html`
- [ ] Find line 551: `const API_BASE_URL = ...`
- [ ] Replace `https://laserostop-backend.onrender.com/api` with YOUR backend URL
- [ ] Example: `https://your-actual-backend.onrender.com/api`
- [ ] Commit and push:
  ```bash
  git add lp_code/index.html
  git commit -m "Update backend URL for production"
  git push
  ```
- [ ] Netlify will auto-redeploy (wait 1-2 minutes)

---

### 4️⃣ Update CORS (Security)

- [ ] On your backend hosting platform (Render/Railway)
- [ ] Update `CORS_ORIGIN` environment variable
- [ ] Change from `*` to your Netlify domain:
  ```
  CORS_ORIGIN=https://your-site.netlify.app
  ```
- [ ] Save and restart service

---

### 5️⃣ Testing

#### Test Backend
- [ ] Open: `https://your-backend-url.com/api/health`
- [ ] Should return: `{"status":"ok","timestamp":"...","tokenCached":false}`
- [ ] Test centers: `https://your-backend-url.com/api/centers`
- [ ] Should return array of 8 centers

#### Test Frontend
- [ ] Open: `https://your-site.netlify.app`
- [ ] Logo appears ✓
- [ ] Map loads with markers ✓
- [ ] Centers dropdown populates (loaded from API) ✓
- [ ] Select a center
- [ ] Calendar displays ✓
- [ ] Select future date
- [ ] Time slots appear ✓
- [ ] Fill form (name, email, phone)
- [ ] Click "Confirmar la cita"
- [ ] Should show success message ✓
- [ ] Open browser console (F12) - no errors ✓

#### Verify Booking in Smart Agenda
- [ ] Go to: https://www.smartagenda.fr/pro/laserostop-esh-dev/agenda
- [ ] Login with credentials
- [ ] Find your test booking in calendar
- [ ] Booking should be there! ✓

---

### 6️⃣ Final Polish (Optional)

- [ ] Custom domain on Netlify (if you have one)
- [ ] Update CORS to custom domain
- [ ] Enable Netlify Analytics
- [ ] Set up uptime monitoring (UptimeRobot)
- [ ] Add Google Analytics (if needed)

---

## 🔐 Security Checklist

- [x] `.env` file in `.gitignore`
- [x] No credentials in frontend code
- [x] Environment variables used in backend
- [ ] CORS restricted to your domain (after deployment)
- [x] HTTPS enabled (automatic on Netlify/Render)

---

## 📝 Important URLs to Save

After deployment, save these URLs:

```
Frontend (Netlify): https://_____________________.netlify.app
Backend (Render):   https://_____________________.onrender.com
Smart Agenda Dev:   https://www.smartagenda.fr/pro/laserostop-esh-dev/agenda
```

---

## ⚠️ Common Issues

### "Cannot connect to backend"
- Check backend URL in index.html line 551
- Verify backend is running (visit /api/health)
- Check browser console for CORS errors

### "CORS policy error"
- Update CORS_ORIGIN in backend to match your Netlify domain
- Include `https://` protocol
- Restart backend service

### "Centers not loading"
- Check browser console (F12)
- Verify backend /api/centers endpoint works
- Check backend logs on Render/Railway

### "Render service sleeping"
- Free tier sleeps after 15 min inactivity
- First request may take 30 seconds to wake up
- Consider keeping it awake with UptimeRobot

---

## 🎉 Success Criteria

Your deployment is successful when:

- ✅ Frontend loads at Netlify URL
- ✅ Centers load from Smart Agenda API
- ✅ Calendar is interactive
- ✅ Can select date and time
- ✅ Form validates correctly
- ✅ Booking creates entry in Smart Agenda
- ✅ No errors in browser console
- ✅ Works on mobile devices

---

## 📞 Need Help?

**Documentation:**
- Full deployment guide: `/lp_code/DEPLOYMENT.md`
- Quick start: `/lp_code/QUICK_START.md`
- Technical docs: `/lp_code/README.md`

**Hosting Documentation:**
- Render: https://render.com/docs
- Netlify: https://docs.netlify.com
- Railway: https://docs.railway.app

---

**Estimated Time:**
- Backend deployment: 15-30 minutes
- Frontend deployment: 10-15 minutes
- Testing: 15 minutes
- **Total: 40-60 minutes**

---

**Current Status:** ✅ **READY TO DEPLOY**

Start with Step 1 above!
