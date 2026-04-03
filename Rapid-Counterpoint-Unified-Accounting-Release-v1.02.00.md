# Rapid POS Unified Accounting Connector v1.02.00 Release Notes

**Release Date:** April 7, 2026

---



---

# QuickBooks Online

## Bug Fixes and Performance Enhancements

- Fixed auto-generation of journal numbers to ensure a unique number is created per date  
  - Prevents errors when the *“Warn me if duplicate journal number is used”* option is enabled  

- Fixed duplicate entries when users retry interfacing after an error  

- Fixed duplicate entries when multiple users perform vouchering at the same time  

- Added validation to prevent conflicts when a vendor name already exists as a customer name  

---

# QuickBooks Desktop

## New Functionality

- Added user-friendly error messaging  
- Introduced pop-up error notifications to improve visibility for users  

- Centralized the database connection in the server directory  
  - Eliminates the need for workstation-level configuration  
  - Reduces risk of corruption
 
## Bug Fixes and Performance Enhancements

- Fixed an issue where `IsErrorRecoveryInfo` could generate duplicate error records across multiple workstations  
  - Now assigns a unique error ID per workstation for accurate tracking  

- Fixed duplicate entries when multiple users perform vouchering simultaneously  

- Fixed duplicate entries when users retry interfacing after an error  
