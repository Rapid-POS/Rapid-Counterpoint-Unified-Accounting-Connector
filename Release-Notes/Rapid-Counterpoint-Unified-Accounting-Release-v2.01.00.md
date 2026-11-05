# Rapid POS Unified Accounting Connector v2.01.00 Release Notes - COMING SOON

**Release Date:** TBD

_This release adds Sage BusinessWorks support and stops duplicate or missing bills when several people voucher to QuickBooks at the same time._

## New Features & Improvements

### Sage BusinessWorks export

You can now send CounterPoint accounting data to Sage BusinessWorks as import files, which you bring in through BusinessWorks' own import screens.

- BusinessWorks doesn't let other programs write to it directly, so nothing posts until you import the files.
- The connector creates three kinds of files: general ledger journal entries, vendor bills and credits (credits come through as adjustments), and new or changed vendors.
- When a file is ready, a "Sage BusinessWorks Import Ready" note in the CounterPoint Message Center tells you where to find it. Problems show up as alerts in a new "SAGEBW Alerts" group, so add at least one person to that group. A pop-up reminds you the first time the connector is installed.
- Each journal entry covers one distribution date, and every line shows the event, package, activity type and document it came from.
- If you turn on the vendor option in setup, the connector creates a vendor file for any vendor that has a voucher and hasn't been sent before. The connector assumes vendors already in CounterPoint at the time of the update are already in BusinessWorks.
- The connector skips any vendor BusinessWorks would reject and lists it in an alert. That covers a missing name, address, city, state or zip code, or an ID longer than 12 characters.
- If CounterPoint's A/P file setting is "always append", every run in a day adds to one file named with the date, and the note says how many lines were added. With any other setting, each run creates its own file named with the date and time.
- A single run can hold up to 999 journal entries, so send larger batches in smaller runs.

### More secure database connection

The connector now signs in to CounterPoint with its own database login and keeps the password encrypted. During the update, you can give the accounts that run the connector access to the encryption key.

### Request timing in the connector log

The connector log now shows how long each QuickBooks Desktop request takes, which makes a slow or locked company file easier to spot.

### New user guides

We added a user guide for the connector and a separate one for Sage BusinessWorks. Both cover daily use, setup and troubleshooting.

## Bug Fixes

### Duplicate or missing QuickBooks Desktop bills when several people voucher at once

When CounterPoint let several users voucher at the same moment, their runs could collide and create duplicate bills or drop some. Now only one run posts at a time. A run that starts while another is posting hands its vouchers over and finishes quickly.

- The connector waits until CounterPoint finishes writing a voucher before posting it, so vouchers don't post half finished.
- The connector clears each voucher from the waiting list as soon as it posts.
- The connector no longer deletes vouchers that other users add while a run is posting.

### QuickBooks Desktop no longer posts the same voucher twice

Each bill and vendor credit now carries a hidden reference to its CounterPoint voucher, and the connector checks for that reference before posting.

- If the voucher is already in QuickBooks, the connector skips it and leaves a note in the Message Center.
- If QuickBooks returns an error, such as a locked company file, the connector checks whether the bill was saved anyway. If it was, the connector counts it as posted instead of retrying and creating a duplicate.
- The connector no longer mistakes vouchers with the same vendor, invoice number and amount for duplicates. This matters if you reuse monthly invoice numbers across locations and purchase orders.
- Bills posted before this update don't carry the reference, so the connector can't match them.

### QuickBooks Online asks you to sign in again when the saved connection stops working

If QuickBooks Online rejected the saved connection (for example, after the connection's administrator changed), the run failed and cleared the waiting accounting data. The connector now catches this before posting and opens the sign-in window. If you don't finish signing in, the run stops and nothing gets cleared.

### Voucher runs no longer fail when an account has no profit center

An account number without a profit center part used to stop the whole voucher run. The connector now treats the profit center as blank and keeps going.

### QuickBooks Online vendor terms due on a day of the month

Vendor terms like "due on the 15th of the month", including their cut-off and discount days, couldn't be created in QuickBooks Online and stopped the run. The connector now creates them correctly.

### Clearer error when a voucher line has no QuickBooks account

If the connector can't read a QuickBooks account from a voucher line's CounterPoint account, the message now names the voucher and tells you what to check. Before, QuickBooks just returned a vague "invalid account" error.

### Correct vendor number and department on QuickBooks Online journal entries

We fixed two labeling problems on journal entries sent to QuickBooks Online.

- The vendor number in a journal line's description now comes from the matching receiver. Before, it could come from a different receiver.
- The connector now sends locations as departments. It had been sending them as classes by mistake.

### Startup errors now reach the log

Errors that happened right at startup could get lost because the program closed before writing them to the log. They now show up in the log file.

## Maintenance

- Updates install more reliably. We fixed installer stops caused by undefined setup values, a column error on some accounting types, and long steps timing out. The installer also no longer adds runtime software the connector doesn't need.

## Known Issues

### Sage BusinessWorks: two voucher runs at the same moment

If two people run the voucher interface for Sage BusinessWorks at exactly the same moment, the same voucher can appear twice in the import file. Check each file before you import it. After importing, move the file to an archive so nobody imports it twice.
