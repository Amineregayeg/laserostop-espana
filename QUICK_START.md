# LaserOstop España - Quick Start Guide

## 🚀 Getting Started in 3 Steps

### Step 1: Start the Backend Server

```bash
cd /mnt/d/LP-espagne/lp_code/backend
npm start
```

You should see:
```
✅ LaserOstop API Server running on http://localhost:3000
📍 Smart Agenda Environment: laserostop-esh-dev (test)
```

### Step 2: Open the Landing Page

**Option A - Through Server (Recommended):**
- Open browser: `http://localhost:3000/lp_code/index.html`

**Option B - Direct File:**
- Open: `/mnt/d/LP-espagne/lp_code/index.html` in browser
- Backend must be running at localhost:3000

### Step 3: Test the Booking Flow

1. **Select a Center** - Choose from dropdown (loads from Smart Agenda)
2. **Pick a Date** - Click any future date on calendar
3. **Choose Time** - Select available time slot
4. **Fill Form** - Enter name, email, phone
5. **Confirm** - Click "Confirmar la cita"

✅ Booking created in Smart Agenda dev environment!

---

## 🧪 Testing Tools

### API Test Page

```
http://localhost:3000/backend/test-api.html
```

Interactive buttons to test each API endpoint:
- Health check
- Load centers
- Load appointment types
- Check availability
- Create test booking

### Manual API Tests

```bash
# Health check
curl http://localhost:3000/api/health

# Get all centers
curl http://localhost:3000/api/centers

# Get appointment types for Valencia
curl http://localhost:3000/api/appointment-types?centerId=1
```

---

## 📋 Verify Bookings in Smart Agenda

1. Go to: https://www.smartagenda.fr/pro/laserostop-esh-dev/agenda
2. Login with dev credentials (see `/smart_agenda/creds.txt`)
3. Navigate to calendar/agenda view
4. Find your test booking by date/time/client name

---

## 🛑 Stopping the Server

```bash
# Find the process
ps aux | grep "node server.js"

# Kill it
kill <PID>

# Or just Ctrl+C in the terminal where it's running
```

---

## ⚠️ Troubleshooting

### Port 3000 already in use

```bash
# Find what's using port 3000
lsof -i :3000

# Kill it
kill -9 <PID>

# Or use a different port
PORT=3001 npm start
```

### Centers not loading

1. Check backend is running: `curl http://localhost:3000/api/health`
2. Open browser console (F12) and check for errors
3. Verify backend console shows "New token obtained successfully"

### Booking fails

1. Check all form fields are filled correctly
2. Verify date is in the future
3. Check backend logs for error messages
4. Test API directly: `curl http://localhost:3000/api/centers`

---

## 📁 Important Files

```
lp_code/
├── index.html              ← Main landing page
├── README.md               ← Full documentation
├── QUICK_START.md          ← This file
└── backend/
    ├── server.js           ← API server
    ├── test-api.html       ← Testing tool
    └── package.json        ← Dependencies
```

---

## 🎯 What's Integrated

✅ **Logo** - LaserOstop branding
✅ **Smart Agenda API** - Real-time booking
✅ **8 Centers** - Dynamically loaded
✅ **Interactive Calendar** - Date selection
✅ **Time Slots** - Available times
✅ **Form Validation** - Input checking
✅ **Booking Creation** - Creates in Smart Agenda
✅ **Spanish Language** - Todo en español

---

## 🔗 Quick Links

- **Frontend:** http://localhost:3000/lp_code/index.html
- **API Test:** http://localhost:3000/backend/test-api.html
- **Smart Agenda:** https://www.smartagenda.fr/pro/laserostop-esh-dev/agenda
- **API Docs:** https://www.smartagenda.fr/pro/laserostop-esh-dev/api/help

---

## 📞 Need Help?

See full documentation:
- `/lp_code/README.md` - Complete setup guide
- `/IMPLEMENTATION_SUMMARY.md` - What was built
- `/SMART_AGENDA_FINDINGS.md` - API details

---

**Status:** ✅ Ready to test!
**Environment:** Development (laserostop-esh-dev)
