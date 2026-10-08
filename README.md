# HRR-CMS Full-Stack v3

**Human Rights Radio — National Human Rights Case Management, Tracking & Records System**

## Major v3 controls

- **Super Administrator only:** create/suspend staff, create teams, add/remove team members, assign/reassign case teams, lock/unlock cases, revoke device sessions and view security center.
- **Case Administrator:** operational case administration and authorized case review, but cannot create staff, manage teams, assign case teams, or lock/unlock cases.
- **Case Officer / Field Officer / other staff:** can create cases and can access only cases they created or cases assigned to them. They have their own dashboard and profile.
- **Admin ↔ Staff messaging:** secure conversation and message module with notifications.
- **Team management:** Add Team opens an on-screen modal form; members are added through an on-screen modal form.
- **Profile/biodata:** full name, phone, state, LGA, address, gender, date of birth, bio and profile photo.
- **Device/login/API tracking:** login events, active device sessions, API request logs and session revocation.
- **Case lock:** only Super Administrator can lock/unlock. Locked cases cannot be edited by ordinary staff or Case Administrators.
- **Case assignment:** only Super Administrator can assign one or more staff members to a case.

## Install

1. Install Node.js and PostgreSQL.
2. Create PostgreSQL database `hrr_cms`.
3. Run `database/schema.sql` and `database/seed.sql`.
4. Copy `.env.example` to `.env` and set PostgreSQL credentials and a strong JWT secret.
5. Run `npm install`.
6. Run `npm start`.
7. Open `http://localhost:4000`.

## Important database note

The schema uses `CREATE TABLE IF NOT EXISTS` plus `ALTER TABLE ... ADD COLUMN IF NOT EXISTS` for the profile additions, so it can be applied to an existing HRR-CMS database. Always back up a production database before applying schema changes.

## First administrator

Use `/api/auth/bootstrap` once when the database has no users, or use the bootstrap flow documented in the previous HRR-CMS version.

## Security

Use HTTPS in production, private evidence storage, backups, strong passwords, least-privilege database access, security-log retention rules, and applicable Nigerian privacy/data-protection requirements.


## Temporary Locked-Case Access

- Staff who are authorized for a locked case can request temporary access.
- The request creates an HRR-CMS chat conversation with the Super Administrator and the requester.
- Only the Super Administrator can approve or reject the request.
- Approval generates a requester-specific temporary access code and sends it through the HRR-CMS chat conversation.
- The code has an administrator-defined expiry (1–1440 minutes).
- The code is single-use and tied to the requesting staff member.
- Redeeming the code creates a temporary case-access grant until the approved expiry time.
- If the case is closed or archived before expiry, all temporary grants and codes for that case are immediately invalidated.
- A later reopening of the case requires a new access request/code; old codes are not restored.
- Direct access-code issuance is disabled; all codes must originate from an approved access request.
