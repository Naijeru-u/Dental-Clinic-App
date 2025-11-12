# Patient Data Handler
A lightweight Python utility module for managing patient data, saving records to JSON files, and automatically archiving inactive patients.

## Overview
The data_handler.py module provides helper functions to:
Load and save patient records from JSON files
Automatically archive inactive patients (not updated in 3 months)
Keep active and archived records organized in separate files

### File Structure
project/
│
├── data_handler.py
├── active_patients.json
└── archived_patients.json

### Functions
1. load_data(file=ACTIVE_FILE)

**Description**:
Loads patient data from a JSON file.

**Returns**:
A list of patient records (or an empty list if the file doesn’t exist or is invalid).

2. save_data(patients, file=ACTIVE_FILE)

**Description**:
Saves patient data into a specified JSON file.

**Parameters**:
* patients — List of patient dictionaries.
* file — Target JSON file path (defaults to active_patients.json).

3. auto_archive_inactive(patients)

**Description**:
Moves patients who haven’t been updated for 3 months (or are marked inactive) into an archive file.

**Logic**:
* Compares the patient’s lastUpdated date to today’s date.
* If more than 90 days old → move to archive.
* Saves both active and archived data automatically.

**Returns**:
Updated list of active patients.

### How It Works
1. Load existing patient data from active_patients.json.
2. Check each record’s lastUpdated field.
3. Automatically move inactive or old records to archived_patients.json.
4. Keep both files synced.

### Example Usage
from data_handler import load_data, save_data, auto_archive_inactive

#Load existing data
patients = load_data()

#Automatically archive inactive patients
active_patients = auto_archive_inactive(patients)

#Add a new patient
new_patient = {
    "name": "Clive Rosfield",
    "lastUpdated": "2025-10-27",
    "isActive": True
}
active_patients.append(new_patient)

#Save updates
save_data(active_patients)

### Archiving Policy
* Inactive period: 90 days
* Archive file: archived_patients.json
* Active file: active_patients.json

### Notes
* If lastUpdated is missing or invalid, the current date is used to avoid accidental archiving.
* The script ensures all JSON files are UTF-8 encoded and properly formatted.

-------------------------------------------------------------------------------------------

# Dental Clinic Management System
A command-line interface (CLI) program for managing patient data in a dental clinic.
The app allows creating, updating, viewing, and archiving patient records — with built-in automation for archiving inactive patients.

## Overview
main.py serves as the entry point of the Dental Clinic Management System.
It integrates multiple modules (data_handler.py, patient_ops.py) to provide a complete system for handling patient information and clinic operations.

### Features
* Create new patient records
* pdate existing patient information
* View active and archived patients
* Track outstanding balances
* View upcoming schedules
* Automatically archive inactive patients (after 3 months)
* Persistent data storage via JSON files

### Main Components
1. data_handler
* Handles all data storage operations:
* Loading and saving patient data
* Automatically archiving inactive patients

2. patient_ops
* Manages user-level operations such as:
* Creating and updating patients
* Displaying lists, stats, and schedules
* Handling user input through the console

### How It Works
1. On startup, main() loads all active patient data.
2. Automatically archives inactive patients (not updated in 3 months).
3. Displays a simple text-based dashboard menu for user actions.
4. Executes the user’s selected option (Create, Update, View, etc.).
5. Refreshes and re-checks data after each operation to ensure consistency.

### Menu Options
======== DENTAL CLINIC CRUD APP ========
1. Create New Patient
2. Update Patient Info
3. View Active Patients
4. View Archived Patients
5. View Floating Balance Stats
6. View Incoming Schedules
7. Exit

### Example Workflow
1. Run the program:
    - python main.py
2. Choose 1 to create a new patient.
3. Use 3 to view active patients.
4. When patients are inactive for over 3 months, they are automatically archived.
5. Exit with 7, which triggers an automatic save of all current data.

### Dependencies
* Python ≥ 3.8
* No external libraries required (uses only Python standard modules)

### Notes
* Data is stored locally in JSON format (active_patients.json, archived_patients.json).
* The app is designed for console use, making it lightweight and portable.
* For best results, ensure all modules (data_handler.py, patient_ops.py, etc.) are in the same directory.