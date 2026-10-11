# EPIC FHIR LDA Pipeline

This repository contains all files used to extract, transform, validate, and visualize
Line, Drains, and Airways (LDA) data from EPIC FHIR. The goal is to keep everything simple,
organized, and easy to use for Power BI dashboards and SQL/Fabric workflows.

## What this repo includes
- **JSON_samples/**  
  Redacted LDA Property and LDA Assessment FHIR JSON files.

- **Spark_Notebooks/**  
  Notebooks used to extract LDA Property and LDA Assessment data into Delta tables.

- **SQL_Views/**  
  SQL logic for combining Property + Assessment into a single analytics-ready view.

- **PowerBI/**  
  PBIX dashboard and DAX measures for visualizing LDA timelines and latest assessments.

## EPIC FHIR LDA Model (Simple Explanation)
EPIC FHIR represents LDAs using two types of Observations:

### 1. LDA Property (Device)
- `Observation.id` = the **device ID**  
- Contains placement date, gauge, orientation, location, and static device details  
- Example: Peripheral IV, CVC, Foley, Fistula

### 2. LDA Assessment (Flowsheet)
- Each assessment is a **separate flowsheet row**  
- Linked to the device using the FHIR extension:  
  `observation-sequelTo → valueReference.reference = Observation/{DeviceID}`  
- Example: Dressing Status, Site Assessment, Line Status, Phlebitis, Infiltration

### Key Concept
**One device → many assessments**  
This is why `Observation.id` stays the same and `FLO_MEAS_ID` changes.

## Power BI Dashboard
The Power BI dashboard uses the combined SQL view to show:
- Device timeline (Property + Assessment)
- Latest device status (Dressing, Site, Line)
- Assessment history matrix
- Multi-patient filtering

## How to Use This Repo
1. Upload JSON samples into `JSON_samples/`
2. Run Spark notebooks to generate Delta tables
3. Use SQL views to create the combined LDA dataset
4. Connect Power BI to the SQL view
5. Build visuals using AssessmentDate, PropertyDate, and LDA labels

## Goal
A clean, simple, and reusable EPIC FHIR LDA pipeline that supports analytics,
quality reporting, and Power BI visualization without complex folder structures.
