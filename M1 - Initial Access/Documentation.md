# Milestone 1 — Initial Access (`medirozahospital.com`)

## 1. Scope, Authorization & Rules of Engagement
- **Target:** `https://medirozahospital.com`
- **Reference:** `WK4 Mediroza v1.pdf` (Authorized lab engagement; testing restricted strictly to `medirozahospital.com` with single requests, no automated scanners/brute-forcing loops, and no denial-of-service traffic).
- **Milestone 1 Objective:** Perform reconnaissance using Kali Linux tools, analyze authentication and input-handling behavior, gain unauthorized access to the patient portal, and retrieve the 3 confidential patient PDF lab reports for Milestone 2.
- **Screenshot Evidence:** `Screenshots/scope-and-authorization.png`

## 2. Reconnaissance & Footprinting (6 Kali Linux Tools)
Executed targeted footprinting against `medirozahospital.com` using six built-in Kali Linux tools to map domain registration, DNS records, web stack, HTTP headers, and edge/WAF posture:

## 2. Reconnaissance & Footprinting (Relevant Tools Only)
Focused reconnaissance on the tools that returned actionable findings for `medirozahospital.com` (omitting tools that yielded no extra signal):

1. **`whois` (Domain Registration & Ownership):**
   - *Findings:* Registered via NameCheap, Inc. (IANA `1068`, ID `3132326963_DOMAIN_COM-VRSN`); created `2026-08-14`, updated `2026-08-14`, expires `2027-08-14`; name servers `DNS1.NAMECHEAPHOSTING.COM` and `DNS2.NAMECHEAPHOSTING.COM`; status `clientTransferProhibited`.
   - *Screenshot evidence:* `Screenshots/whois-medirozahospital.png`
2. **`whatweb` (Server & Redirect Behavior):**
   - *Findings:* Resolved to `199.188.201.16` (`US`), `LiteSpeed` (`HTTPServer[LiteSpeed]`, `PoweredBy[LiteSpeed]`, `x-turbo-charged-by`, `HTML5`); `http://medirozahospital.com` returned `301` to `https://medirozahospital.com/`, which returned `403 Forbidden` to `whatweb`.
   - *Screenshot evidence:* `Screenshots/whatweb-medirozahospitaL.png`
3. **`curl` (HTTP Headers, Sessions & Endpoints):**
   - *Findings:* Confirmed `PHP/8.2.36` over `LiteSpeed` (`Mediroza CMS 1.4.2`), inspected `PHPSESSID` and `Cache-Control` headers, mapped `/patient/login.php` and `/patient/portal.php` (`.report-row`), and verified unauthenticated portal redirects back to `/patient/login.php`.
   - *Screenshot evidence:* `Screenshots/curl-medirozahospitaL.png`

### Exposed Entry Points & Session Behavior Identified
- **Authentication Form:** `/patient/login.php` (`POST` accepting `username` and `password`).
- **Protected Portal:** `/patient/portal.php` (displays `.report-row` lab report entries when authenticated; redirects unauthenticated `GET` requests and unauthenticated `PHPSESSID` states back to `/patient/login.php`).
- **Screenshot evidence:** `Screenshots/portal-redirect.png`
  `Screenshots/incorrect-password.png`


## 3. Pre-Access Analysis & Response Baselines
Before achieving portal access, single-request observations on `/patient/login.php` established distinct response baselines:
- **Standard Failure Responses (`2281` bytes, `HTTP/2 200 OK`):**
  - Unfound user test cases returned `<div class="alert">Username not found</div>`.
  - Existing user test cases returned `<div class="alert">Incorrect password</div>`, indicating separate user lookup and credential verification paths.
  - *Screenshot evidence:* `Screenshots/09-username-not-found.png`
    `Screenshots/incorrect-password.png`
- **Verbose SQL Error Responses (`2451`–`2471` bytes, `HTTP/2 200 OK`):**
  - Certain single-quote (`'`) inputs triggered `Warning: mysqli_query(): You have an error in your SQL syntax...` (such as `near '' OR '1'='1'' at line 1` at `2471` bytes), revealing single-quote string framing around `username` and unparameterized query construction.
  - *Screenshot evidence:* `Screenshots/syntax-error.png`
- **Non-Error / Non-Matching Variants:** Inputs that executed without a syntax error returned `2281`-byte alerts until a valid session transition occurred.
  - *Screenshot evidence:* `Screenshots/incorrect-password.png`

## 4. Access Achievement & Retrieved Files (Handoff to M2)
- **Access Proof:** Obtained an authenticated session (`PHPSESSID`) and accessed `/patient/portal.php` (`200 OK`) showing `.report-row` entries.
  - *Screenshot evidence:* `Screenshots/portal-access-report.png`
- **Retrieved Lab Reports:** Downloaded all 3 target report files locally for **Milestone 2** encryption/hash analysis:
  - *File 1:* `<filename_1>` | `SHA-256: <sha256_1>` | `Retrieved: <timestamp_utc>` | *Screenshot:* `screenshots/13-file1-download-hash.png`
  - *File 2:* `<filename_2>` | `SHA-256: <sha256_2>` | `Retrieved: <timestamp_utc>` | *Screenshot:* `screenshots/14-file2-download-hash.png`
  - *File 3:* `<filename_3>` | `SHA-256: <sha256_3>` | `Retrieved: <timestamp_utc>` | *Screenshot:* `screenshots/15-file3-download-hash.png`
- *(Note: Redact or omit raw patient personal identifiable information in shared or public repository notes per lab/privacy rules.)*

## 5. Remediation Recommendations
- **Prepared Statements:** Refactor `/patient/login.php` database queries to use parameterized prepared statements (`mysqli_prepare` with bound parameters) instead of string concatenation.
- **Error Handling & Generic Auth Responses:** Disable verbose `mysqli` / PHP error display in production and return uniform, non-enumerating authentication messages.
- **Session Enforcement:** Verify authenticated session state at the top of `/patient/portal.php` and all report-retrieval endpoints before serving records.
- *Screenshot evidence:* `screenshots/16-remediation-summary.png`
