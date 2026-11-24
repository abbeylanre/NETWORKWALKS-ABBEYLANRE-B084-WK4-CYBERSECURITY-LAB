# Milestone 2 — Data Extraction (`medirozahospital.com`)

## 1. Scope, Authorization & File Handling Hygiene
- **Target Context:** 3 confidential patient PDF lab reports retrieved during Milestone 1 (`https://medirozahospital.com`, per `WK4 Mediroza v1.pdf`).
- **Authorization & Rules:** Conducted strictly within authorized lab scope on local files only; no external or unauthorized target activity.
- **Tooling Used:** Online hash calculator and Networkwalks hash calculator for hash extraction; Networkwalks Password Crack for initial password recovery on PDF 1 and PDF 2, followed by `hashcat` with `rockyou.txt` on Kali Linux for PDF 3 after the built-in 100-password list did not match.
- **Repository Hygiene:** Raw PDF files, unredacted patient PII, and sensitive details remain local outside the repository; only sanitized methodology, metadata, and proof screenshots are documented here.
- **Screenshot Evidence:** [The 3 Patient Report PDF](./Screenshots/patient-report.png)

## 2. File 1 — Encryption Analysis & Recovery
- **File Reference:** Patient Report 1 / PDF 1 (`<local_file_1_label>`, stored locally; excluded from repository)
- **Hash Extraction (`Online Hash Calculator` / `Networkwalks Hash Calculator`):** Extracted the hash value using the online hash calculator and Networkwalks hash calculator (`<sanitized hash format or observation>`).
- **Password Recovery (`Networkwalks Password Crack`):** Submitted the extracted hash value to Networkwalks Password Crack and recovered the password (`123456`) within the built-in wordlist.
- **Recovery Outcome:** **Cracked** — confirmed local access to PDF 1 (`<sanitized summary of recovered contents without exposing raw PII or sensitive credentials>`).
- **Screenshot Evidence:** [File1 Hash Value](./Screenshots/file1-hash.png) and [Evidence of File1 Access](./Screenshots/file1-access.png)

## 3. File 2 — Encryption Analysis & Recovery
- **File Reference:** Patient Report 2 / PDF 2 (`<local_file_2_label>`, stored locally; excluded from repository)
- **Hash Extraction (`Online Hash Calculator` / `Networkwalks Hash Calculator`):** Extracted the hash value using the online hash calculator and Networkwalks hash calculator (`<sanitized hash format or observation>`).
- **Password Recovery (`Networkwalks Password Crack`):** Submitted the extracted hash value to Networkwalks Password Crack and recovered the password (`password`) within the built-in wordlist.
- **Recovery Outcome:** **Cracked** — confirmed local access to PDF 2 (`<sanitized summary of recovered contents without exposing raw PII or sensitive credentials>`).
- **Screenshot Evidence:** [File2 Password Cracked](./Screenshots/file2-password.png) and [Evidence of File2 Access](./Screenshots/file2-access.png)

## 4. File 3 — Encryption Analysis & Recovery
- **File Reference:** Patient Report 3 / PDF 3 (`<local_file_3_label>`, stored locally; excluded from repository)
- **Hash Extraction (`Online Hash Calculator` / `Networkwalks Hash Calculator`):** Extracted the hash value using the online hash calculator and Networkwalks hash calculator (`<sanitized hash format or observation>`).
- **Initial Attempt & Adaptation (`Networkwalks Password Crack` → Kali `hashcat` + `rockyou.txt`):** Initial submission to Networkwalks Password Crack returned *"Access denied / Not cracked with this wordlist"* because the candidate (`!@#$%^&`) was outside the built-in 100-password list. Adapted by running `hashcat` on Kali Linux against the extracted hash using the larger `rockyou.txt` wordlist to recover the password (`!@#$%^&`).
- **Recovery Outcome:** **Cracked (via Kali `hashcat` + `rockyou.txt`)** — recovered the password after expanding beyond the 100-password list, confirming local access to PDF 3 (`<sanitized summary of recovered contents without exposing raw PII or sensitive credentials>`).
- **Screenshot Evidence:** [File3 Password Cracked](./Screenshots/file3-password.png) and [Evidence of File3 Access](./Screenshots/file3-access.png)

## 5. Cross-File Observations & Handoff to Milestone 3
- **Comparison Across All 3 Files:** PDF 1 (`123456`) and PDF 2 (`password`) were recovered directly through Networkwalks Password Crack's built-in 100-password list, whereas PDF 3 (`!@#$%^&`) used a special-character string that required pivoting to `hashcat` with `rockyou.txt` on Kali Linux.
- **Leads & Properties for Milestone 3:** `<Sanitized observations or file properties relevant to Milestone 3 data exposure analysis (staff salaries and shareholder details)>`
