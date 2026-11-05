# Rapid POS Unified Accounting Connector for QuickBooks Desktop v2.00.09 Release Notes - Coming Soon

**Release Date:** TBD

_This release stops duplicate bills when several users voucher at once and keeps one bad account number from halting a voucher run._

## New Features & Improvements

### Request timing in the connector log

The connector log now shows how long each QuickBooks request takes, so a slow or locked company file is easier to spot.

## Bug Fixes

### Duplicate bills when several users voucher at the same time

We fixed an issue where a voucher could post to QuickBooks Desktop twice when more than one user vouchered receivings at the same time. Each voucher now posts exactly once.

- Only one voucher run posts at a time. If another user starts a run during that time, it hands its vouchers to the active run and closes without an error.
- Each bill and vendor credit now carries a hidden reference to its CounterPoint voucher. If a voucher is already in QuickBooks, the connector skips it, including after a QuickBooks timeout or lock error.
- Bills posted before this update don't have the reference, so the connector can't match them.

### Voucher run stopping on an incomplete account number

We fixed an issue where one voucher line with an incomplete account number stopped the entire voucher run. The connector now treats the missing part as blank and keeps going.

- If the connector can't read a QuickBooks account from a line at all, the run stops with a message naming the voucher and the settings to check.
