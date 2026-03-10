# Rapid POS Unified Accounting Connector v1.02.00 Release Notes
Release Date: March 2026

## Bug Fixes and Enhancements

### QuickBooks Online – Journal Number Generation
Fixed the auto-generation of journal numbers to ensure a unique number is created per date. This prevents errors when the **“Warn me if duplicate journal number is used”** option is enabled.

### QuickBooks Desktop – Database Connection Location
The database connection is now centralized in the **server directory** and no longer needs to be added to the **workstation top-level directory**, which could sometimes become corrupted.

### QuickBooks Desktop – Error Recovery Tracking
Fixed an issue where **IsErrorRecoveryInfo** could generate duplicate error records across multiple workstations. The system now assigns a **unique error ID per workstation** to ensure accurate error recovery tracking and prevent duplicate entries.

### QuickBooks Desktop – Duplicate Voucher Entries
Resolved an issue that could cause **duplicate entries when multiple users were vouchering at the same time**.
