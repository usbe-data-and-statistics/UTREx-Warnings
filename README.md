# UTREx-Warnings

A Python-based utility for Utah LEAs to compare **UTREx Level 1 and Level 2 validation files**. This script identifies newly appearing warnings between two files while ignoring "noisy" data changes like fluctuating attendance numbers or membership days.

## Features

* **Smart Cleaning:** Automatically handles data type mismatches, removes `.0` suffixes from IDs, and strips whitespace/line breaks.
* **Noisy Data Filtering:** Ignores changes in columns that update daily (e.g., `DaysAttended`, `GPA`, `Membership`) so they don't trigger "false" new warnings.
* **Dual Mode Operation:**
    * **Mode 1:** Compare the two most recent UTREx validation files.
    * **Mode 2:** Compare the latest UTREx validation file against a previously generated "New Warnings" report.
* **Excel Integration:** Preserves sheet structures for Level 1 and Level 2 errors and exports a clean, multi-sheet `.xlsx` report.

## Prerequisites

You must have Python 3.x installed. The script also requires the `pandas`, `openpyxl`, and `xlsxwriter` libraries.

## Setup

Place the script in a dedicated folder.
Ensure your UTREx validation files are in the same folder.
File Naming: Your raw data files must follow the naming pattern:
`UTREx-Level1AndLevel2-Validations-*.xlsx`

## How to Use

Run the jupyter notebook in an IDE of your choice

Select Mode:
Mode 1 (Fresh Run): Select this to compare the two most recent UTREx validation files in your folder.
Mode 2 (Returning Run): Select this to compare the latest UTREx validation file against the last report you generated (`New_Warnings_Report_...`). This is useful for tracking changes over time.

Review Output: The script will generate a new file named `New_Warnings_Report_YYYYMMDD.xlsx` containing only the new warnings found.

## Comparison Logic

Identifying "New" Warnings
The script uses a composite key to match records across files. It only uses the column in the key if it is available:

`ErrorMessage`
`StudentNumber`
`LEANumber`
`LeaNumber`
`SchoolNumber`
`EntryDate`
`ScramEntryDate`
`YICEntryDate`
`StatewideStudentID`
`IncidentID`
`CourseRecordID`
`otherLEANumber`
`otherSchoolNumber`
`otherEntryDate`
`otherScramEntryDate`

Columns that are ignored:

`CollectTS`
`DateRecorded`

Handling "Noisy" Data
To prevent warnings from showing up as "new" simply because a student's attendance changed by one day, the script automatically updates the "old" data with the "new" values for the following fields before performing the comparison:

`SchoolMembership`
`DaysAttended`
`ExcusedAbsences`
`UnexcusedAbsences`
`AbsencesDueToSuspension`
`ScramMembership`
`YICMembership`
`YICMembershipActual`
`ActualMembership`

Extended descriptions for `S1.383w`, `S1.329w`, and Level 2 `04` warning

Columns that might not change daily but *can change are not ignored* (e.g., `GPA`, `CreditsAttemped`, `CreditsEarned`, Address fields, etc.)

## As-Is/Open-Source

Since this isn't a commercial product, we won’t be providing official tech support, bug fixes, or future updates. If the UTREx file format is changed, the script might need a tweak.

Please take this code, use it, and adapt it to your own LEA's needs.
