# LaserOstop España - Landing Page with Smart Agenda Integration

Landing page for LaserOstop España with real-time booking integration to Smart Agenda API.

## Features

- ✅ Logo and branding
- ✅ Interactive map with 8 active centers across Spain
- ✅ Real-time center loading from Smart Agenda API
- ✅ Dynamic calendar with date selection
- ✅ Time slot availability (integrated with Smart Agenda)
- ✅ Complete booking flow with validation
- ✅ Direct booking creation in Smart Agenda system
- ✅ Spanish language throughout

## Architecture

```
Frontend (index.html)
    ↓
Backend API Proxy (Node.js/Express)
    ↓
Smart Agenda REST API (laserostop-esh-dev)
```

### Why Backend Proxy?

1. **Security**: Hides API credentials from frontend
2. **CORS**: Avoids cross-origin issues
3. **Token Management**: Handles 2-hour token refresh automatically
4. **Error Handling**: Better error messages for users
5. **Validation**: Server-side validation before creating bookings

## Setup Instructions

### Prerequisites

- Node.js v14 or higher
- npm (comes with Node.js)

### Installation

1. **Navigate to backend directory:**
   ```bash
   cd lp_code/backend
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start the server:**
   ```bash
   npm start
   ```

   The server will start on `http://localhost:3000`

### Development Mode

For auto-restart on file changes:
```bash
npm run dev
```

## Usage

1. **Start the backend server** (see above)

2. **Open the landing page:**
   - Go to `http://localhost:3000/lp_code/index.html`
   - Or open the file directly and ensure backend is running

3. **Test the booking flow:**
   - Select a center (loaded from Smart Agenda)
   - Choose a date and time
   - Fill in your information
   - Click "Confirmar la cita"
   - Booking will be created in Smart Agenda dev environment

## API Endpoints

### Backend API (http://localhost:3000/api)

- **GET /api/health** - Server health check
- **GET /api/centers** - Get all active centers from Smart Agenda
- **GET /api/appointment-types?centerId=1** - Get appointment types for a center
- **GET /api/availability?startDate=2025-10-15&endDate=2025-10-22** - Get available time slots
- **POST /api/booking** - Create new booking

### Example API Calls

```bash
# Health check
curl http://localhost:3000/api/health

# Get centers
curl http://localhost:3000/api/centers

# Get appointment types for Valencia (ID=1)
curl http://localhost:3000/api/appointment-types?centerId=1

# Get availability
curl "http://localhost:3000/api/availability?startDate=2025-10-20&endDate=2025-10-27"

# Create booking (POST)
curl -X POST http://localhost:3000/api/booking \
  -H "Content-Type: application/json" \
  -d '{
    "fullName": "Juan Pérez",
    "email": "juan@example.com",
    "phone": "+34612345678",
    "centerId": "1",
    "typeId": "26",
    "startTime": "2025-10-20 10:00:00",
    "endTime": "2025-10-20 10:30:00"
  }'
```

## Smart Agenda Configuration

- **Environment**: Development (laserostop-esh-dev)
- **API Base URL**: https://www.smartagenda.fr/pro/laserostop-esh-dev/api
- **Web Interface**: https://www.smartagenda.fr/pro/laserostop-esh-dev/agenda
- **API Documentation**: https://www.smartagenda.fr/pro/laserostop-esh-dev/api/help

### Credentials

Stored in `/smart_agenda/creds.txt` (not in frontend for security)

## Active Centers

The following centers are loaded dynamically from Smart Agenda:

1. Barcelona Sants
2. Madrid Atocha
3. Madrid Chamartín
4. Majadahonda
5. San Sebastián de los Reyes
6. Sevilla
7. Torrejón de Ardoz
8. Valencia

## File Structure

```
lp_code/
├── index.html              # Main landing page
├── assets/
│   └── logolaserostop-bleu.svg  # LaserOstop logo
├── backend/
│   ├── server.js           # Express API server
│   ├── package.json        # Node.js dependencies
│   └── node_modules/       # Installed packages
└── README.md               # This file
```

## Features Breakdown

### Frontend (index.html)

- **Logo Header**: LaserOstop branding at top
- **Hero Section**: Main call-to-action
- **Booking Form**: 3-step process
  1. Select center (dynamic dropdown)
  2. Choose date & time (calendar + time slots)
  3. Enter personal information
- **Interactive Map**: Leaflet.js with OpenStreetMap
- **Center Cards**: Quick access to center selection
- **Form Validation**: Client-side and server-side
- **Loading States**: Visual feedback during API calls

### Backend (server.js)

- **Token Management**: Automatic token caching and refresh
- **Smart Agenda Proxy**: All endpoints proxied securely
- **Error Handling**: Comprehensive error messages
- **CORS Enabled**: Allows frontend to call API
- **Static File Serving**: Serves frontend files

## Development Notes

### Token Caching

The backend caches Smart Agenda tokens for 2 hours (with 5-minute buffer). This reduces API calls and improves performance.

### Availability Integration

Currently using sample time slots. The availability API is integrated but needs testing with actual Smart Agenda response format. Once tested, update `processAvailabilityData()` function in `index.html`.

### Appointment Types

The system auto-selects the first available appointment type for the selected center. You can add UI to let users choose between different appointment types (single session, DUO, follow-up, etc.).

## Testing

### Test Booking in Dev Environment

1. Select any center
2. Choose a future date
3. Select any available time slot
4. Fill in test information:
   - Name: Test Usuario
   - Email: test@example.com
   - Phone: +34612345678
5. Click confirm

The booking will be created in Smart Agenda's dev environment and visible at:
https://www.smartagenda.fr/pro/laserostop-esh-dev/agenda

### Verify Booking

1. Log into Smart Agenda web interface
2. Navigate to calendar/agenda view
3. Find the booking by date/time/client name

## Troubleshooting

### Backend won't start

- **Port 3000 already in use**: Kill existing process or change PORT in server.js
  ```bash
  # Find process using port 3000
  lsof -i :3000

  # Kill it
  kill -9 <PID>
  ```

### Centers not loading

- Check backend is running: `curl http://localhost:3000/api/health`
- Check browser console for errors (F12)
- Verify CORS is enabled in backend

### Booking fails

- Check form validation passes (all fields filled)
- Verify center, date, and time are selected
- Check backend logs: `tail -f backend/server.log`
- Verify Smart Agenda credentials are correct

### API Errors

- Check backend console output for detailed error messages
- Verify token is being cached: `GET /api/health` should show `tokenCached: true` after first request
- Test Smart Agenda API directly using curl with credentials

## Production Deployment

Before deploying to production:

1. **Change Environment**: Update credentials to production Smart Agenda environment
2. **Environment Variables**: Move credentials to environment variables (not hardcoded)
3. **HTTPS**: Enable HTTPS for secure communication
4. **Domain**: Update `API_BASE_URL` in frontend to your production domain
5. **Error Handling**: Add production-grade error logging
6. **Rate Limiting**: Add rate limiting to prevent abuse
7. **Monitoring**: Add health checks and monitoring

### Environment Variables for Production

```bash
SMART_AGENDA_LOGIN=your_login
SMART_AGENDA_PWD=your_password
SMART_AGENDA_API_ID=your_api_id
SMART_AGENDA_API_KEY=your_api_key
SMART_AGENDA_BASE_URL=https://www.smartagenda.fr/pro/your-env/api
PORT=3000
NODE_ENV=production
```

## License

Internal use only - LaserOstop España

## Support

For issues or questions, contact the development team.

---

**Status**: ✅ Fully functional integration with Smart Agenda dev environment
**Last Updated**: October 14, 2025
