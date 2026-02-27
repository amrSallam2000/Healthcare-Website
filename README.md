### **O'verio Healthcare Website – Comprehensive Documentation**

#### **Project Overview**
O'verio is a large-scale digital transformation platform that automated the complete patient journey—from registration to medication dispensing—for a major healthcare facility. It replaced a chaotic, manual-first operation with a structured, rule-based system that ensures fairness, transparency, and operational efficiency while serving over 50,000 active patients.

## Link ##
-- http://mnfhosp.com/Default.aspx

**Technologies:** ASP.NET Web Forms, C#, MS SQL Server, HTML/CSS/JavaScript, IIS




---

#### **1. Core Problems & Solutions**

| Problem Area | Old Process | O'verio Solution | Impact |
|--------------|-------------|------------------|---------|
| **Medication Chaos** | Dawn queues, no guarantees, stock issues | Tiered scheduling by category/rank + dual slots per patient | Zero queues, fair distribution |
| **Clinic Booking** | Physical visits/unanswered calls, no visibility | **Rolling 2-week window** + rank-based days + 4-booking limit | 24/7 access, fair distribution across ranks |
| **Patient Data** | Incomplete records, missing National IDs | Mandatory fields + data merge tools + registration portal | 100% data integrity |
| **Communication** | No way to notify patients of changes | **Phone number collection for future SMS integration** | Foundation laid for notifications |
| **Administration** | All-powerful users, no reports, manual adds | RBAC + dashboards + bulk import + admin override | Controlled, auditable operations |

---

#### **2. System Use Cases**

**2.1. Medication Dispensing Management**
- **Primary Actor:** Patient, Pharmacist
- **Preconditions:** Patient has valid prescription, belongs to scheduled category/rank
- **Flow:**
  1. System assigns dispensing month by category (Officers: even months, Enlisted: odd months)
  2. Patient logs in during assigned week, sees available slots
  3. Patient selects slot (primary or catch-up)
- **Business Rules:**
  - No same-day booking for same medication type
  - Catch-up slot only accessible if primary slot missed
  - Admin can override for emergency cases

**2.2. Clinic Appointment Booking - ENHANCED**
- **Primary Actor:** Patient
- **Preconditions:** Patient registered, has available booking quota (<4 active) per month
- **Flow:**
  1. **Weekly Window Logic:** System opens booking for **current week + next week only** (2-week rolling window)
  2. Patient selects specialty clinic
  3. System shows **only available slots within the 2-week window**, filtered by:
     - Patient's rank-specific days
     - Clinic capacity per rank
     - Holiday blocks
  4. Patient selects date/time from available slots
  5. **Advanced Validation:**
     - No duplicate booking in same week for same clinic
     - Max 4 active bookings total per patient in month
     - Slot not already taken by same rank quota
  6. Booking confirmed, generates downloadable ticket image
  7. Patient can cancel (requires National ID double verification)
- **Why 2-Week Window?**
  - **Fairness:** Prevents "booking hoarding" by early birds
  - **Predictability:** Patients know exactly when to plan
  - **Load Balancing:** Even distribution across all ranks weekly
  - **Emergency Flexibility:** Admin can open additional weeks if needed

**2.3. Patient Registration & Data Integrity**
- **Primary Actor:** New Patient, Admin
- **Flow:**
  1. New patient submits request via public portal (Name, National ID, Phone)
  2. Admin reviews, checks against existing records
  3. If duplicate suspected, uses Merge Tool to consolidate
  4. Admin approves, system activates account
  5. Patient receives credentials, can immediately book
- **Data Validation:**
  - National ID format and uniqueness check
  - **Phone number collected for future communication capabilities**
  - Mandatory fields enforced

**2.4. Role-Based Access Control (RBAC)**
| Role | Permissions | Access Scope |
|------|-------------|--------------|
| **Admin** | All privileges, override rules, bulk operations | Full system |
| **Pharmacist** | view patient medication history | Pharmacy module only |
| **User** | View own bookings, update contact info, |accept and reject request | , | Personal dashboard only |

**2.5. Reporting & Dashboard**
- **Real-time Metrics:** Active bookings, dispensing volumes, registration requests
- **Operational Reports:** Clinic utilization,  patient attendance
- **Administrative:** Audit logs, user activity, override records

---

#### **3. Key Transactions & Business Logic**

| Transaction | Rules & Validation | Output |
|-------------|-------------------|---------|
| **Medication Slot Allocation** | Category→Month→Rank→Day logic | Fair schedule for 50K+ patients |
| **Clinic Booking** | **2-week window** + rank-based days + quota check + duplicate prevention | **Weekly fair distribution** across all ranks |
| **Patient Registration** | National ID validation + duplicate check + phone collection | Complete patient profiles ready for future integrations |
| **Window Management** | Automatically rolls forward weekly, preserves existing bookings | Continuous availability without overload |
| **Admin Override** | Can modify window, open additional weeks (logged) | Flexible emergency handling |

---

#### **4. System Architecture Overview**

```
[Frontend] → ASP.NET Web Forms (UI Layer)
       ↓
[Business Logic] → C# (Scheduling Engine, RBAC, Validation Rules)
       ↓
[Data Access] → Stored Procedures / Entity Framework
       ↓
[Database] → MS SQL Server (Patient, Booking, Medication, Audit Tables)
       ↓
[Integration Points] → **Ready for SMS Gateway**, Excel Import/Export, Report Server
```

**Scalability Features:**
1. **Modular Communication Layer** – Phone numbers stored in standardized format
2. **Event Logging** – Schedule changes logged for potential notification triggers
3. **API-Ready Structure** – Can integrate SMS/Email services when needed

---

#### **5. Performance & Impact Metrics**

| Metric | Result (First 9 Months) | Business Value |
|--------|-------------------------|----------------|
| Total Bookings Processed | ~50,000 | Handled 300% more than manual system |
| New Registration Requests | ~10,000 | Streamlined digital onboarding |
| Active Patient Profiles | ~50,000 | Complete organizational coverage |
| **Booking Fairness** | **Even weekly distribution** | Eliminated rank-based grievances |
| **Data Completeness** | **100% National IDs, 95% phone numbers** | Foundation for future automations |
| System Availability | >99.5% | Mission-critical reliability |

---

#### **6. Design Decisions & Future-Proofing**

**1. Communication Infrastructure (Planned but Paused)**
- **Implemented:** Phone number collection at registration
- **Architected:** Database fields and logging for notification events
- **Client Decision:** SMS integration postponed due to budget/priority
- **System Readiness:** Can activate within 2 weeks when approved

**2. The Fairness Algorithm**
```plaintext
EVERY MONDAY 00:01:
   Close: Week-2 (becomes historical)
   Open: Week+2 (new future week)
   
FOR EACH PATIENT:
   IF Rank = Officer → Show Mon/Wed/Fri slots
   IF Rank = Enlisted → Show Tue/Thu/Sat slots
   
   ENFORCE:
   - Max 4 total bookings
   - No same clinic in 7 days
   - Not on holiday schedule
```

**3. Admin Flexibility vs. System Rigidity**
- **Strict Rules:** For patients → ensures fairness
- **Admin Override:** For emergencies → ensures practicality
- **Audit Trail:** All overrides logged → ensures accountability

---

#### **7. Lessons Learned & Best Practices**

1. **Fairness > Features:** A simple 2-week window solved more problems than complex features
2. **Data First:** Collecting complete data upfront enables future enhancements
3. **Admin Tools Matter:** Invest in override capabilities—real-world exceptions always occur
4. **Rolling Updates:** Weekly window reset became a predictable ritual users appreciated
5. **Performance at Scale:** SQL Server + stored procedures handled 50k+ records effortlessly

---

✅ **Conclusion**
O'verio succeeded by focusing on **core workflows first**: fair scheduling, complete data, and administrative control. While communication features were architected for future implementation, the system delivered immediate value by solving the most critical pain points: chaotic queues and unfair access. The platform stands as a testament to **pragmatic software design**—building what's essential today while preparing for tomorrow's needs.
