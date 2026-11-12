# Rapid POS Unified Accounting Connector v2.01.00 Release Notes

**Release Date:** November 15, 2026

_This release adds a Sage BusinessWorks connector, keeps QuickBooks Online signed in on its own, makes QuickBooks Desktop vouchering safe when several people voucher at once, and adds menu actions to clear stuck staged entries._

## New Features & Improvements

### Sage BusinessWorks connector

You can now send CounterPoint GL entries and A/P vouchers to Sage BusinessWorks as import files, ready to bring in through BusinessWorks' own import screens.

- GL journal entries and A/P invoices and credits each get their own import file. A Message Center alert tells you when a file is ready and which BusinessWorks menu to import it from.
- The connector writes vendors to their own file the first time a voucher needs them. Import that file before the A/P invoice file.
- If a vendor is missing something BusinessWorks requires, such as an address or ZIP code, the connector skips it and lists it in Message Center. Once you fix the vendor, the connector picks it up again.
- When CounterPoint's A/P file setting is "always append", the connector adds A/P entries to one file per day instead of creating a new file per run. Message Center reminds you to move the file after you import it.
- Vendor credits come in as open vendor credits, with the CounterPoint invoice number in the Reference field.
- Each GL line description shows its event, package, activity type and document number.
- BusinessWorks limits the size of each import file, so the connector stops any batch with more than 999 journal entries and asks you to split it.
- Nothing posts automatically. Someone has to import each file in BusinessWorks.

### QuickBooks Online stays signed in on its own

The connector now saves your QuickBooks Online sign-in securely with your company's accounting settings and renews it at the start of each run. It no longer depends on which Windows user signed in.

- The sign-in window only opens when QuickBooks stops accepting the saved sign-in.
- Two runs started together, such as distributions and vouchers, no longer interfere with each other's sign-in renewal.

### Clear stuck staged entries from the CounterPoint menu

When entries get stuck waiting to be sent, you can now use **Clear Staged A/P Vouchers** and **Clear Staged GL Distributions** under **Connectors > Accounting**.

- Clearing deletes the staged entries without sending them. Anything that was never sent has to be vouchered again.
- The connector won't clear A/P vouchers while a voucher run is posting, so it never pulls entries out from under a run. Try again after that run finishes.
- The update adds these menu entries to the CounterPoint menus set up for your company.

### Clearer message when a voucher line has no account

When QuickBooks Desktop vouchering can't read an account from a voucher line, the alert now names the voucher and the account and tells you which accounting interface settings to check. Before, QuickBooks only reported a vague invalid-account error.

## Bug Fixes

### Duplicate or missing QuickBooks Desktop bills when several people voucher at once

With CounterPoint's concurrent vouchering turned on, two people vouchering at the same moment could post a bill twice or lose one. The connector now posts vouchers one run at a time and never sends a voucher twice.

- If a run starts while another is posting, it hands its vouchers to the run that is already working, so CounterPoint isn't left waiting.
- The connector checks every voucher against QuickBooks first. If the voucher is already there, the connector skips it and Message Center says so.
- After a QuickBooks error or timeout, the connector asks QuickBooks whether the bill or credit was saved anyway, so it doesn't post it a second time.
- If CounterPoint is still saving a voucher, the connector waits for the next pass instead of sending it half-complete.
- The connector clears each voucher from staging as soon as it posts, so CounterPoint can stage that voucher again if it needs to.

### QuickBooks Online no longer clears staged entries when sign-in is rejected

If QuickBooks Online rejected the saved sign-in, the run could fail and still clear the staged entries without posting them. The connector now signs in again when needed. If you close the sign-in window without finishing, it stops with a Message Center alert and leaves your entries staged.

### QuickBooks Desktop no longer skips real vouchers that share an invoice number

The connector could mistake vouchers that reuse an invoice number, such as a monthly "AUG 26" across locations and POs, for duplicates and skip them. It now tells vouchers apart by their PO, invoice date, memo and lines.

### Vouchers no longer fail on accounts without every segment

If an account on a voucher line had no profit center (or another segment), the whole voucher run stopped. The connector now treats that segment as empty and keeps going.

### QuickBooks Online vendor terms with a day of the month

Creating a QuickBooks Online vendor term due on a day of the month used to fail. The connector now creates these terms correctly, with their cutoff and early-payment discount. Net-days terms now include their early-payment discount days too.

### QuickBooks Online journal entry details

- Lines for received merchandise now have the correct vendor number in their description.
- The connector now tags locations as locations (departments) instead of classes.

## Maintenance

- Updates on servers are more reliable. Database steps get more time, the update skips .NET runtime installs it doesn't need, and we fixed setup errors on some accounting types.
- Updates give the Windows accounts that run the connector access to the secure key. They can also prepare a password-protected key package for moving the key to a workstation.
- The connector now connects to CounterPoint with a dedicated login that the update sets up, instead of reading Companies.ini. If your server serves several companies, Rapid sets up a separate install folder for each company during the update, so you don't need to do anything.
- If the connector stops at startup, the error details now always reach the log file.

## Known Issues

### One-time QuickBooks Online sign-in after updating

The connector now saves the sign-in with your company's accounting settings instead of under a Windows user, so the first run after the update asks you to sign in to QuickBooks Online again. Run the connector from a desktop session and complete the sign-in window. Later runs renew it automatically.
