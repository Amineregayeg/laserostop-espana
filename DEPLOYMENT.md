# LaserOstop España - Deployment Guide

## 🚀 Quick Deployment Steps

### Prerequisites
- GitHub account
- Netlify account (free)
- Render/Railway/Heroku account (free tier available)

---

## Step 1: Backend Deployment (Render.com - Recommended)

### Option A: Deploy to Render.com (Free Tier)

1. **Create Render account**: https://render.com

2. **Create New Web Service**:
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Select the `lp_code/backend` directory

3. **Configure Service**:
   ```
   Name: laserostop-backend
   Region: Frankfurt (EU) or closest to Spain
   Branch: main
   Root Directory: lp_code/backend
   Runtime: Node
   Build Command: npm install
   Start Command: npm start
   Instance Type: Free
   ```

4. **Add Environment Variables**:
   Go to "Environment" tab and add:
   ```
   SMART_AGENDA_BASE_URL=https://www.smartagenda.fr/pro/laserostop-esh-dev/api
   SMART_AGENDA_LOGIN=eshapiFG42dmPs87JxX
   SMART_AGENDA_PWD=f3be0da94b09f33ae362fa92a069508c50c67150
   SMART_AGENDA_API_ID=app_landing
   SMART_AGENDA_API_KEY=84Pm-92tQ-49Rt24LSpe2C
   NODE_ENV=production
   PORT=3000
   CORS_ORIGIN=*
   ```

5. **Deploy**: Click "Create Web Service"

6. **Get Your Backend URL**:
   - After deployment: `https://laserostop-backend.onrender.com`
   - Copy this URL for Step 2

---

### Option B: Deploy to Railway.app

1. **Create Railway account**: https://railway.app

2. **New Project**:
   - Click "New Project"
   - "Deploy from GitHub repo"
   - Select your repository

3. **Configure**:
   - Root Directory: `lp_code/backend`
   - Start Command: `npm start`

4. **Add Variables**: Same as Render above

5. **Generate Domain**: Railway will give you a URL

---

### Option C: Deploy to Heroku

```bash
# Install Heroku CLI
# https://devcenter.heroku.com/articles/heroku-cli

cd /mnt/d/LP-espagne/lp_code/backend

# Login
heroku login

# Create app
heroku create laserostop-backend

# Set environment variables
heroku config:set SMART_AGENDA_BASE_URL=https://www.smartagenda.fr/pro/laserostop-esh-dev/api
heroku config:set SMART_AGENDA_LOGIN=eshapiFG42dmPs87JxX
heroku config:set SMART_AGENDA_PWD=f3be0da94b09f33ae362fa92a069508c50c67150
heroku config:set SMART_AGENDA_API_ID=app_landing
heroku config:set SMART_AGENDA_API_KEY=84Pm-92tQ-49Rt24LSpe2C
heroku config:set NODE_ENV=production

# Deploy
git push heroku main
```

---

## Step 2: Frontend Deployment (Netlify)

### Deploy to Netlify

1. **Create Netlify account**: https://www.netlify.com

2. **Deploy via Git**:
   - Click "Add new site" → "Import an existing project"
   - Choose "Deploy with GitHub"
   - Select your repository
   - Configure build settings:
     ```
     Base directory: lp_code
     Build command: (leave empty)
     Publish directory: .
     ```

3. **Deploy**: Click "Deploy site"

4. **Get Your Frontend URL**:
   - Example: `https://laserostop-espana.netlify.app`
   - You can customize this in Site settings → Domain management

5. **Update Frontend API URL**:
   - Go back to your code
   - Edit `lp_code/index.html` line 551
   - Replace `https://laserostop-backend.onrender.com/api` with your actual backend URL
   - Commit and push changes
   - Netlify will auto-redeploy

---

## Step 3: Update CORS

Once you have your Netlify domain:

1. **Update Backend Environment Variables**:
   - On Render/Railway/Heroku dashboard
   - Change `CORS_ORIGIN` from `*` to your Netlify domain:
     ```
     CORS_ORIGIN=https://laserostop-espana.netlify.app
     ```

2. **Restart Backend**: Service will restart automatically

---

## Step 4: Test Production Deployment

1. **Open your Netlify URL**: `https://your-site.netlify.app`

2. **Test booking flow**:
   - Select a center
   - Choose date and time
   - Fill form
   - Submit booking

3. **Verify in Smart Agenda**:
   - Login: https://www.smartagenda.fr/pro/laserostop-esh-dev/agenda
   - Check booking appears

4. **Check browser console** (F12) for any errors

---

## GitHub Setup

### Initialize Git Repository

```bash
cd /mnt/d/LP-espagne/lp_code

# Initialize git (if not already)
git init

# Add files
git add .

# Commit
git commit -m "Initial commit: LaserOstop landing page with Smart Agenda integration"

# Create GitHub repo (via GitHub website)
# Then:
git remote add origin https://github.com/YOUR_USERNAME/laserostop-espana.git
git branch -M main
git push -u origin main
```

---

## Environment Variables Reference

### Backend (.env)

```env
# Smart Agenda API
SMART_AGENDA_BASE_URL=https://www.smartagenda.fr/pro/laserostop-esh-dev/api
SMART_AGENDA_LOGIN=eshapiFG42dmPs87JxX
SMART_AGENDA_PWD=f3be0da94b09f33ae362fa92a069508c50c67150
SMART_AGENDA_API_ID=app_landing
SMART_AGENDA_API_KEY=84Pm-92tQ-49Rt24LSpe2C

# Server
NODE_ENV=production
PORT=3000

# Security
CORS_ORIGIN=https://your-netlify-site.netlify.app
```

---

## Post-Deployment Checklist

- [ ] Backend deployed and accessible
- [ ] Environment variables set correctly
- [ ] Frontend deployed to Netlify
- [ ] Frontend API_BASE_URL points to backend
- [ ] CORS configured with Netlify domain
- [ ] Test booking creates entry in Smart Agenda
- [ ] All 8 centers load correctly
- [ ] Calendar works
- [ ] Form validation works
- [ ] Confirmation message appears
- [ ] Check browser console for errors
- [ ] Test on mobile device

---

## Monitoring & Maintenance

### Backend Monitoring

**Render.com**:
- Dashboard → Logs
- View real-time logs
- Check for errors

**Uptime Monitoring** (free tools):
- UptimeRobot: https://uptimerobot.com
- Pingdom: https://www.pingdom.com

### Frontend Monitoring

**Netlify**:
- Dashboard → Deploys (see all deployments)
- Dashboard → Functions (if using)
- Analytics (if enabled)

---

## Troubleshooting

### Backend Issues

**Service won't start**:
- Check environment variables are set
- View logs on hosting platform
- Verify all npm packages installed

**CORS errors**:
- Update `CORS_ORIGIN` to match your Netlify URL
- Include https:// protocol
- Restart backend service

### Frontend Issues

**Can't connect to backend**:
- Check `API_BASE_URL` in index.html
- Verify backend is running
- Check browser console for errors

**Centers not loading**:
- Open Network tab in DevTools
- Check API calls to /api/centers
- Verify backend logs

---

## Updating Production

### Backend Updates

```bash
# Push changes to GitHub
git add .
git commit -m "Update backend"
git push

# Render/Railway auto-deploys from GitHub
# Or redeploy manually in dashboard
```

### Frontend Updates

```bash
# Update index.html
# Commit and push
git add .
git commit -m "Update frontend"
git push

# Netlify auto-deploys
```

---

## Custom Domain (Optional)

### Add Custom Domain to Netlify

1. Go to Site settings → Domain management
2. Click "Add custom domain"
3. Enter your domain (e.g., laserostop.es)
4. Follow DNS configuration instructions
5. Netlify provides free SSL certificate

### Update Backend CORS

Update `CORS_ORIGIN` to your custom domain

---

## Costs

- **Render Free Tier**: $0/month (sleeps after 15 min inactivity)
- **Railway Free Tier**: $5 credit/month
- **Heroku Free Tier**: Deprecated (use Render/Railway)
- **Netlify Free Tier**: 100GB bandwidth/month
- **GitHub**: Free for public repos

**Total Cost**: $0/month with free tiers

---

## Production vs Development

### Switch to Production Smart Agenda

When ready for production, update environment variables:

```env
SMART_AGENDA_BASE_URL=https://www.smartagenda.fr/pro/YOUR-PROD-ENV/api
SMART_AGENDA_LOGIN=your_prod_login
SMART_AGENDA_PWD=your_prod_pwd
# ... etc
```

---

## Support

For deployment issues:
- Render: https://render.com/docs
- Netlify: https://docs.netlify.com
- GitHub: https://docs.github.com

---

**Ready to deploy!** Start with Step 1 above.
