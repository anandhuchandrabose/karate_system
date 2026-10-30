# KARATE CLUB MANAGEMENT SYSTEM
## Comprehensive Development Handoff for Codex

**Document:** Product & Technical Handoff  
**Version:** 1.0  
**Status:** Requirements defined, implementation not yet started  
**Primary focus:** Multi-club management and configurable event registration  
**Expected event scale:** 1,000+ participants per event

---

# 1. CORE OBJECTIVE

## 1.1 Project Goal

Build a centralised Karate Club Management System that allows an organisation to manage multiple karate clubs, instructors, students, and sporting events through two separate role-based dashboards.

The application must provide:

1. An Admin Dashboard for centralised organisation management.
2. An Instructor Dashboard for managing assigned clubs and students.
3. A fully configurable Event Management System.
4. Individual and bulk Excel-based event registration.
5. Centralised event participant management and Excel exports.

The most important product feature is Event Management.

The system must make it easy for instructors to register hundreds of participants without repetitive manual entry or requiring administrators to approve every registration individually.

The application should be designed as a scalable management platform, not simply a generic CRUD dashboard.

## 1.2 Primary User Roles

### Admin

The admin manages the complete organisation.

Responsibilities:
- Create and manage clubs.
- Create and manage instructors.
- Create and manage permanent student profiles.
- Assign instructors to clubs.
- Assign instructors to students.
- Create and configure events.
- Configure event registration forms.
- Configure event categories and category codes.
- View all event registrations.
- Edit and cancel registrations.
- Export event participant lists.
- View organisation-wide reports.

### Instructor

The instructor manages assigned clubs and students.

Responsibilities:
- View assigned clubs.
- View assigned students.
- Add permanent students.
- Edit permitted student details.
- View student profiles.
- View available events.
- Register existing students.
- Register external participants manually.
- Upload Excel files for bulk registration.
- View registrations submitted by them.
- Edit registrations when permitted.
- Export their registration lists.

## 1.3 Important Product Distinction: Two Types of Participants

The system must distinguish between permanent students and event-only participants.

**Permanent Student**
- Exists in the main student database.
- Has a unique student ID.
- Belongs to a club.
- Can be assigned to an instructor.
- Appears in normal student management.
- Can participate in multiple events.

**Event-only Participant**
- Does not exist in the permanent student database.
- Is registered manually or through Excel.
- Exists only as a participant in the relevant event.
- Does not receive a permanent student ID.
- Does not appear in the normal student directory.
- Is not automatically assigned to an instructor as a permanent student.

An unmatched participant from an Excel upload must also remain event-only unless explicitly matched to an existing student.

This distinction is a mandatory business rule.

## 1.4 Main Product Modules

### Admin Dashboard
1. Dashboard Overview
2. Club Management
3. Instructor Management
4. Student Management
5. Assignment Management
6. Event Management
7. Event Registrations
8. Reports
9. Notifications
10. Settings

### Instructor Dashboard
1. Dashboard Overview
2. My Clubs
3. My Students
4. Add Student
5. Student Profiles
6. Events
7. Individual Registration
8. Excel Bulk Registration
9. My Registrations
10. Notifications
11. My Profile

---

# 2. ARCHITECTURE & DESIGN DECISIONS

## 2.1 Technology Stack

The preferred frontend stack is:

- React
- Vite
- TypeScript
- Tailwind CSS

The user has previously expressed a preference for Tailwind UI and avoiding Material UI.

Important: The complete backend stack, database technology, authentication strategy, and deployment infrastructure have not yet been finalised.

Codex should inspect the repository and recommend the backend architecture before implementation.

Do not assume that a particular backend or database has already been selected.

## 2.2 Application Architecture

Use a modular architecture.

The application should separate:
- Authentication and authorisation.
- Admin functionality.
- Instructor functionality.
- Club management.
- Instructor management.
- Student management.
- Event configuration.
- Individual registration.
- Excel import and validation.
- Participant management.
- Reporting and exports.

Keep business logic separate from UI components.

Avoid putting all application logic inside large page components.

Use reusable components for:
- Forms.
- Tables.
- Search.
- Filters.
- Modals.
- Drawers.
- Confirmation dialogs.
- Pagination.
- File uploads.
- Import previews.
- Status indicators.

## 2.3 Role-Based Access Control

The system has two initial roles:

ADMIN
INSTRUCTOR

Admin:
- Organisation-wide access.

Instructor:
- Access to assigned clubs.
- Access to permitted students.
- Access to their own registration submissions.
- No permission to modify event configurations.
- No access to unrelated clubs' private student information.

Access control must be enforced in the backend, not only by hiding frontend navigation items.

## 2.4 Club and Instructor Relationships

The organisation supports multiple clubs.

Confirmed capabilities:
- An admin can create multiple clubs.
- An instructor can be assigned to clubs.
- An instructor can add students through their dashboard.
- Admin can assign instructors to students.
- Admin can reassign students.

Whether an instructor can simultaneously belong to multiple clubs and whether a student can have multiple instructors should be configurable according to the final organisation rules. Do not hardcode assumptions without confirming them.

## 2.5 Event Management Architecture

Event creation must be configurable rather than hardcoded for specific karate event types.

The same event management interface must support:
- Championships.
- Belt examinations.
- Seminars.
- Workshops.
- Training camps.
- Other custom events.

Use a configurable form builder and configurable category system.

Event-specific fields and categories should be controlled by admin.

Avoid creating a separate registration workflow for every event type.

### Event Configuration

Admin can configure:

**Basic information**
- Event name.
- Description.
- Event type.
- Banner.
- Venue.
- Start date.
- End date.
- Registration opening date.
- Registration closing date.

**Participation**
- All clubs or selected clubs.
- Individual registration enabled/disabled.
- Excel upload enabled/disabled.
- Manual event-only registration enabled/disabled.

**Categories**
- Category name.
- Category code.
- Gender.
- Age group.
- Competition type.
- Weight category.
- Other custom eligibility fields.

**Age configuration**
- Optional minimum and maximum age.
- Configurable age calculation date.

**Registration form**
- Text.
- Number.
- Date.
- Dropdown.
- Single selection.
- Multiple selection.
- Checkbox.
- File upload.

Admin controls field labels, required status, order, and options.

### Reusable Event Templates

Admin should be able to duplicate existing events.

Duplicating an event should copy its configuration:
- Form fields.
- Categories.
- Eligibility rules.
- Registration settings.

The new event must be independent of the original event.

## 2.6 Registration Architecture

Every event provides two registration methods.

### Method 1: Individual Registration

Two options:

A. Existing student.

1. Search by name or student ID.
2. Select student.
3. Automatically populate available profile details.
4. Display additional event-specific fields.
5. Instructor enters missing event details.
6. Submit.

B. Manual entry.

1. Enter participant information.
2. Fill event-specific fields.
3. Select applicable categories.
4. Submit.

Manually entered external participants must remain event-only.

### Method 2: Excel Bulk Registration

Instructors can upload `.xlsx` files containing large participant lists.

Required functionality:
- Download event-specific Excel template.
- Upload Excel file.
- Detect column headings.
- Map columns to configured event fields.
- Preview imported data.
- Validate individual rows.
- Detect duplicate registrations.
- Validate category codes.
- Validate age eligibility when configured.
- Import valid records.
- Report invalid records.
- Download error reports.
- Track import history.

### Registration Processing Rules

There is no individual admin approval process.

Successful registration submissions should immediately become registered participants.

Payment processing and payment verification are outside the application's scope.

The operating assumption is that instructors register only students whose payments have already been collected.

The application must not implement payment management in Version 1.

### Duplicate Prevention

For existing students:
- Identify the student using their permanent student ID.
- Prevent duplicate registrations for the same event and category.
- Permit registration in multiple distinct categories.

For event-only participants:
- Detect potential duplicates using participant information and category.
- Flag ambiguous matches.
- Do not automatically merge uncertain identities.

Repeated Excel uploads must not insert the same registrations again.

### Registration Status

Keep initial registration statuses simple:
- Registered.
- Cancelled.

Do not introduce pending approval or payment statuses.

## 2.7 Excel Import Processing

Bulk import is a critical feature.

The application must support events containing 1,000+ registrations.

Recommended processing flow:

1. File selection.
2. File validation.
3. Column detection.
4. Column mapping if required.
5. Data preview.
6. Row-level validation.
7. Duplicate checking.
8. Import valid records.
9. Generate import summary.
10. Download error report.

Example:

Uploaded rows: 1,000  
Valid rows: 960  
Failed rows: 40

The 960 valid rows should be importable without waiting for the remaining 40 to be corrected.

Use safe, reliable import processing. Avoid inserting the entire spreadsheet as one uncontrolled operation.

Preserve successfully imported rows when other rows fail.

For large files, consider backend batch processing and pagination rather than sending all records in one large request.

---

# 3. IMPLEMENTATION PLAN / PROGRESS

## 3.1 Current Project Status

No application implementation has been confirmed in this conversation.

The work completed so far is product discovery and functional requirements definition.

Confirmed:
- Product goal.
- Two primary user roles.
- Admin responsibilities.
- Instructor responsibilities.
- Club management requirements.
- Student management requirements.
- Instructor assignment requirements.
- Configurable event creation.
- Individual registration.
- Excel bulk registration.
- Event-only participant handling.
- Duplicate prevention requirements.
- No individual admin approval.
- No event payments.
- No event attendance.
- Future features intentionally excluded from Version 1.

Not yet completed:
- Repository inspection.
- Backend selection.
- Database selection.
- Authentication implementation.
- API definitions.
- Database schema.
- UI implementation.
- Excel import implementation.
- Automated tests.
- Deployment.

## 3.2 Phase 0: Repository Inspection and Planning

Before coding:

1. Inspect the existing repository.
2. Identify existing dependencies.
3. Check whether the project is empty or partially implemented.
4. Identify existing UI components and styling.
5. Identify current environment and configuration.
6. Recommend a suitable backend and database.
7. Propose a modular folder structure.
8. Identify security requirements.
9. Present an implementation roadmap.

Do not overwrite an existing project without inspecting it.

Do not generate the entire application in one step.

## 3.3 Phase 1: Foundation

Implement:
- Project setup.
- Authentication.
- Role-based access.
- Admin layout.
- Instructor layout.
- Sidebar navigation.
- Responsive dashboard layouts.
- Reusable UI components.
- Protected routes.

## 3.4 Phase 2: Organisation Management

Implement:
- Club CRUD.
- Instructor CRUD.
- Student CRUD.
- Instructor-to-club assignment.
- Instructor-to-student assignment.
- Student search and filtering.
- Student profile pages.

Verify access permissions for each role.

## 3.5 Phase 3: Event Creation

Implement:
- Event CRUD.
- Event types.
- Configurable event settings.
- Registration field builder.
- Category builder.
- Category codes.
- Age eligibility configuration.
- Event duplication.
- Event publishing and closure.

This phase should produce a working event configuration interface before registration is implemented.

## 3.6 Phase 4: Individual Registration

Implement:
- Existing student search.
- Automatic profile population.
- Event-specific fields.
- Manual participant entry.
- Event-only participant handling.
- Category selection.
- Duplicate prevention.
- Registration confirmation.
- Instructor registration history.

## 3.7 Phase 5: Excel Bulk Registration

This phase is a high priority.

Implement:
- Event-specific template generation.
- `.xlsx` upload.
- Existing SKAT template compatibility.
- Header detection.
- Column mapping.
- Preview.
- Row-level validation.
- Category validation.
- Age validation.
- Duplicate detection.
- Partial import.
- Import summaries.
- Error-only export.
- Import history.

Test using realistic files containing hundreds and thousands of rows.

## 3.8 Phase 6: Event Administration

Implement:
- Consolidated participant list.
- Search and filters.
- Club-wise reports.
- Instructor-wise reports.
- Category-wise reports.
- Registration editing.
- Cancellation.
- Excel exports.

## 3.9 Phase 7: Testing and Stabilisation

Test:
- Role permissions.
- Student assignment.
- Existing student registration.
- External participant registration.
- Duplicate detection.
- Different event configurations.
- Excel imports.
- Partial failures.
- Large participant lists.
- Responsive layouts.
- Data integrity.

## 3.10 Future Development

Keep these outside the initial release:

- Student and parent portals.
- Event payments.
- Event attendance.
- QR check-in.
- Tournament bracket generation.
- Match scheduling.
- Competition scoring.
- Examination results.
- Automatic belt progression.
- Certificate generation.
- Advanced analytics.
- Detailed event database extensions.

The initial architecture should avoid unnecessary blockers to these future features, but they must not expand Version 1 scope.

---

# 4. TECHNICAL CONSTRAINTS & RULES

## 4.1 Frontend Styling

Preferred:
- React.
- Vite.
- TypeScript.
- Tailwind CSS.

Styling preferences:
- Use Tailwind CSS.
- Prefer reusable Tailwind-based components.
- Do not use Material UI.
- Avoid MUI modals.
- Maintain a clean, professional dashboard design.
- Ensure responsive layouts.
- Use consistent typography, spacing, colours, and component patterns.

Do not introduce additional UI libraries without evaluating whether they are necessary.

## 4.2 Performance Requirements

The application should:
- Support 1,000+ participants per event.
- Paginate large data tables.
- Use efficient server-side filtering and searching.
- Avoid unnecessary frontend rendering.
- Process Excel imports reliably.
- Prevent duplicate records during retries.
- Provide useful feedback during long-running imports.

Do not load thousands of participant records into a single uncontrolled frontend table.

## 4.3 Excel Compatibility

Reference files supplied by the user:

1. `SKAT_CADET_JUNIOR_U21_2026.xlsx`
2. `SKAT_CADET_JUNIOR_U21_CATEGORIES_2026.pdf`

The Excel file is titled:

TVPM DISTRICT 14 TO 20 (CADET, JUNIOR & U21) KARATE CHAMPIONSHIP 2026

Its actual header row is row 3:

| Column | Header |
|---|---|
| A | SL NO |
| B | NAME |
| C | DOB |
| D | GENDER |
| E | PARENT |
| F | WEIGHT |
| G | KA CODE |
| H | TKA CODE |
| I | KU CODE |
| J | CLUB |
| K | INSTRUCTOR |

Rows 1 and 2 contain the championship title and organising information.

Important: The spreadsheet is a formatted template, not a simple flat CSV-style file. Inspect its actual worksheet structure, merged cells, formatting, and dimensions before implementing a parser.

Support this original template as a recognised import format, while also generating event-specific templates from the configurable form builder.

An optional STUDENT ID column should be supported to identify existing students reliably.

Do not rely exclusively on participant names for identity matching.

## 4.4 Category Reference PDF

The supplied PDF is titled:

SPORTS KARATE ASSOCIATION OF THIRUVANANTHAPURAM (SKAT)  
District Karate Championship 2026 - Age as on 31-12-2026

It contains:

### Cadet / Junior
- Age range: 14–17.
- Boys and girls categories.
- Kata.
- Team Kata.
- Kumite weight categories.

### Under 21
- Age range: 18–20.
- Male and female categories.
- Kata.
- Kumite weight categories.

The reference contains 56 Cadet/Junior categories and 15 Under-21 categories, totalling 71 listed categories.

Examples:
- KA 13: Boys Kata 14 Years.
- TKA 101: Junior Boys Team Kata (14–17 Years).
- KU 25: Boys Kumite 14 Years -45KG.
- KU 71: Male Kumite Under 21 Years -50KG.

Age must be calculated as of the event-configured date.

For the reference championship, that date is 31-12-2026.

The category list must not be hardcoded into the application. It should be configurable per event.

## 4.5 Important Business Rules

1. External participants remain event-only.
2. No automatic permanent student creation.
3. No individual admin approval.
4. No payment processing.
5. No event attendance.
6. Existing students should have unique student IDs.
7. Registration identity must be preserved.
8. Students may participate in multiple categories.
9. Duplicate event/category registrations must be prevented.
10. Event forms must be configurable.
11. Category definitions must be configurable.
12. Age cutoff must be configurable.
13. Excel import must support partial success.
14. Excel retries must not duplicate existing registrations.
15. Historical event registrations must not change simply because a student's current club or instructor assignment changes.

## 4.6 Security Requirements

- Backend-enforced role permissions.
- Secure authentication.
- Protected admin and instructor routes.
- Input validation.
- File type and file size validation.
- Safe Excel parsing.
- Restricted file access.
- No exposure of unrelated club/student data.
- Activity history for important changes.

## 4.7 Features to Avoid in Version 1

Do not build:
- Payment gateway.
- Payment verification.
- Payment approval.
- Event attendance.
- QR event check-in.
- Individual admin registration approvals.
- Tournament brackets.
- Certificate automation.
- Student/parent portal.

These are explicitly postponed.

---

# 5. CODE SNIPPETS / KEY STATE / DATA DEFINITIONS

## 5.1 Finalised Code

No production code, API signatures, database schema, authentication implementation, or function signatures have been finalised in the discussion.

Do not treat illustrative UI examples from the planning conversation as existing code.

Codex must inspect the actual repository before defining implementation structures.

## 5.2 Conceptual Domain Entities

The following are product-level entities, not a final database schema.

Organisation management:
- User
- Club
- Instructor
- Student
- InstructorClubAssignment
- InstructorStudentAssignment

Event management:
- Event
- EventCategory
- EventFormField
- EventRegistration
- RegistrationAnswer
- EventImportHistory

The final database schema must be proposed after repository inspection and architecture planning.

The user explicitly postponed detailed event database architecture. Do not over-engineer future payment, attendance, results, and certificate entities in Version 1.

## 5.3 Conceptual Registration Structure

A registration must conceptually identify:

- Event.
- Participant type.
- Existing student reference, when applicable.
- Participant details.
- Club association, when applicable.
- Instructor who submitted the registration.
- Selected categories.
- Custom form responses.
- Registration status.
- Submission timestamp.

Participant type:

- EXISTING_STUDENT
- EVENT_ONLY

For existing students, preserve a reference to the permanent student profile.

For event-only participants, store participant information in the event registration context without creating a permanent student profile.

The implementation must decide how to preserve registration-time snapshots of participant and club information.

## 5.4 Conceptual Excel Import Summary

An import operation should return:

- Import ID.
- Event ID.
- File name.
- Uploaded by.
- Total rows.
- Valid rows.
- Imported rows.
- Failed rows.
- Duplicate rows.
- Row-level error information.
- Import timestamp.

Example response shape (illustrative, not a finalized API contract):

{
  "totalRows": 1000,
  "validRows": 960,
  "importedRows": 960,
  "failedRows": 40,
  "duplicateRows": 0,
  "errors": [
    {
      "row": 12,
      "field": "KU CODE",
      "message": "Invalid category code"
    }
  ]
}

The final API contract should be defined after the backend stack is selected.

## 5.5 Conceptual Registration Validation

Validation should cover:

- Required fields.
- Data types.
- Date formats.
- Gender options.
- Category codes.
- Age eligibility.
- Duplicate registrations.
- Existing student matching.
- Event registration deadlines.

The validation engine should use event configuration rather than hardcoded field names.

## 5.6 Key Import Behaviour

For a spreadsheet with valid and invalid rows:

- Valid rows should be importable.
- Invalid rows should be reported.
- Failed rows should be downloadable.
- Corrected rows should be uploadable again.
- Already imported rows should not be duplicated.
- Existing student matches should retain their student identity.
- Unmatched participants should remain event-only.

---

# 6. INSTRUCTIONS TO CODEX

You are responsible for implementing this product incrementally.

Follow these instructions:

1. Treat this document as the authoritative Version 1 product specification.
2. Inspect the existing repository before modifying anything.
3. Do not assume the project is empty.
4. Do not assume a backend or database has already been selected.
5. Do not generate the entire application in one response.
6. Do not invent missing business requirements.
7. Identify important ambiguities before implementation.
8. Keep confirmed requirements separate from technical recommendations.
9. Follow the preferred React, Vite, TypeScript, and Tailwind CSS frontend stack if compatible with the existing repository.
10. Avoid Material UI.
11. Prioritise clean architecture, reusable components, and role-based permissions.
12. Treat Excel bulk registration as a core product feature, not an optional utility.
13. Test with the supplied SKAT reference files.
14. Prioritise correct event registration and participant identity handling.
15. Do not introduce excluded features into Version 1.

## First Task: Planning Only

Before writing application code:

A. Inspect the repository.

B. Report:
- Existing framework.
- Existing dependencies.
- Current folder structure.
- Existing implementation.
- Configuration and environment.
- Any issues or missing pieces.

C. Recommend:
- Backend technology.
- Database technology.
- Authentication approach.
- API architecture.
- Frontend folder structure.
- Backend folder structure.
- Excel parsing and validation approach.

D. Explain how permanent students and event-only participants will be distinguished.

E. Present the development roadmap and milestones.

F. Identify any unresolved product decisions that genuinely block implementation.

Wait for confirmation of the proposed architecture before beginning major implementation.

**Primary success criterion:** A reliable, easy-to-use karate management platform in which instructors can manage their assigned students and register 1,000+ event participants efficiently through individual registration or bulk Excel uploads, while administrators retain centralised control of clubs, students, instructors, and events.
