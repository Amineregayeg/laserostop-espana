# Smart Agenda Support Email Template

---

**To**: Smart Agenda Support
**From**: LaserOstop España Technical Team
**Subject**: API `/pdo_type_rdv` Missing Centers 6, 8, 9, 10 on Account `laserostop-esh-dev`
**Priority**: High
**Account**: laserostop-esh-dev

---

## Issue Summary

The Smart Agenda API endpoint `/pdo_type_rdv` is not returning appointment types for 4 out of 8 centers, despite these types being visible and correctly configured in the dashboard.

---

## Environment Details

- **Account**: laserostop-esh-dev
- **Dashboard URL**: https://www.smartagenda.fr/pro/laserostop-esh-dev/agenda
- **API Base URL**: https://www.smartagenda.fr/pro/laserostop-esh-dev/api
- **Endpoint Affected**: `/pdo_type_rdv`

---

## Problem Description

### What We See in Dashboard ✅

The prestations configuration page shows **32 appointment types** correctly configured:
- 8 centers × 4 appointment types per center = 32 total
- All types have correct durations, prices, and configuration

**Centers visible in dashboard**:
1. Valencia (ID: 1) - 4 types
2. Barcelona Sants (ID: 4) - 4 types
3. Sevilla (ID: 5) - 4 types
4. **Torrejón (ID: 6) - 4 types** ❌
5. Madrid Chamartín (ID: 7) - 4 types (partial)
6. **Madrid Atocha (ID: 8) - 4 types** ❌
7. **San Sebastián (ID: 9) - 4 types** ❌
8. **Majadahonda (ID: 10) - 4 types** ❌

### What API Returns ❌

When calling `GET /pdo_type_rdv` with authentication token:

**Centers 1, 4, 5, 7**: Return appointment types (with some legacy duplicates)
**Centers 6, 8, 9, 10**: Return **0 appointment types**

---

## Specific Missing Appointment Types

### Torrejón (Center ID: 6)
- ID 53: Dejar de fumar - Solo (60min, €190)
- ID 56: Dejar de fumar - Duo (90min, €170)
- ID 57: En caso de recaída (30min, €0)
- ID 59: Adicción al cannabis (60min, €250)

### Madrid Atocha (Center ID: 8)
- ID 63: Dejar de fumar - Solo (60min, €190)
- ID 65: Dejar de fumar - Duo (90min, €170)
- ID 66: En caso de recaída (30min, €0)
- ID 68: Adicción al cannabis (60min, €250)

### San Sebastián (Center ID: 9)
- ID 81: Dejar de fumar - Solo (60min, €190)
- ID 83: Dejar de fumar - Duo (90min, €170)
- ID 84: En caso de recaída (30min, €0)
- ID 86: Adicción al cannabis (60min, €250)

### Majadahonda (Center ID: 10)
- ID 72: Dejar de fumar - Solo (60min, €190)
- ID 74: Dejar de fumar - Duo (90min, €170)
- ID 75: En caso de recaída (30min, €0)
- ID 77: Adicción al cannabis (60min, €250)

---

## Timeline

- **When created**: 2+ hours before this ticket
- **Current status**: Dashboard shows types, API does not return them
- **Expected behavior**: API should sync with dashboard within 15-30 minutes
- **Actual behavior**: API still returns stale data after 2+ hours

---

## Troubleshooting Already Attempted

1. ✅ **Generated fresh authentication tokens** - Same result
2. ✅ **Waited 2+ hours for cache refresh** - No change
3. ✅ **Verified center IDs match** - Correct IDs confirmed via `/pdo_groupe` endpoint
4. ✅ **Checked appointment type configuration** - All visible and correct in dashboard
5. ✅ **Restarted API client** - New token obtained, same stale data

---

## Questions

1. **Why does `/pdo_type_rdv` not return appointment types for centers 6, 8, 9, 10?**

2. **Is there an API cache that needs manual purging for our tenant?**

3. **Are there visibility flags specific to API export** (separate from dashboard visibility)?
   - For example: "Show on website" or "Bookable online" flags that only affect API?

4. **Does `/pdo_type_rdv` require additional query parameters** to return all types?
   - Example: `?id_groupe=6` or `?include_all=true`

5. **Could there be archived/legacy centers** polluting the results?
   - We see unexpected center IDs (-1, 0, 2, 3) in API response
   - How can we exclude these at the API level?

6. **Can you verify these appointment type IDs exist in your database?**
   - Torrejón: IDs 53, 56, 57, 59
   - Madrid Atocha: IDs 63, 65, 66, 68
   - San Sebastián: IDs 81, 83, 84, 86
   - Majadahonda: IDs 72, 74, 75, 77

7. **Is there a multi-agenda grouping** that affects how `/pdo_type_rdv` returns data?

---

## Impact

### Business Impact: CRITICAL

- **50% of our centers are non-functional** via our booking website
- Cannot deploy production website to customers
- Customers trying to book at Torrejón, Madrid Atocha, San Sebastián, or Majadahonda see 0 available appointment types
- This blocks our entire online booking system launch

### Current Workaround

We have implemented a temporary frontend workaround (hardcoded appointment types) to proceed with launch, but we need the API fixed for long-term maintainability.

---

## Requested Resolution

1. **Investigate why `/pdo_type_rdv` is not returning types for centers 6, 8, 9, 10**
2. **Purge API cache if necessary** for our tenant
3. **Verify database consistency** for the missing appointment type IDs
4. **Provide guidance on correct API usage** if we're missing required parameters
5. **Confirm timeline for resolution**

---

## API Example Request

```bash
curl -X POST https://www.smartagenda.fr/pro/laserostop-esh-dev/api/token \
  -H 'Content-Type: application/json' \
  -d '{
    "login": "eshapiFG42dmPs87JxX",
    "pwd": "f3be0da94b09f33ae362fa92a069508c50c67150",
    "api_id": "app_landing",
    "api_key": "84Pm-92tQ-49Rt24LSpe2C"
  }'

# Returns token, then:

curl https://www.smartagenda.fr/pro/laserostop-esh-dev/api/pdo_type_rdv \
  -H 'X-SMARTAPI-TOKEN: <token>'

# Expected: 32 appointment types
# Actual: ~50 types (includes legacy), missing centers 6, 8, 9, 10
```

---

## Contact Information

**Account**: laserostop-esh-dev
**Environment**: Production (laserostop-esh-dev)
**Response Needed By**: ASAP (blocking production launch)

We appreciate your prompt attention to this matter.

---

**Prepared**: 2025-10-17
**Status**: Awaiting Smart Agenda support response
