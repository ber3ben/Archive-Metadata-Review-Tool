# Archive-Metadata-Review-Tool
A tool that can read and pull Internet Archive metadata for review to complete audits. Identifies missing fields, dates, duplicates, whitespace, etc.

### Features
**1. Flexible Data Retrieval** - Fetches metadata via IA advanced search or direct item endpoints
**2. Comprehensive Validation** - Detects missing fields, date issues, duplicates, and type inconsistencies
**3. Smart Date Handling** - Normalizes ISO 8601 timestamps and validates date ranges
**4. Severity Classification** - Issues categorized as HIGH, MEDIUM, or LOW priority
**5. CSV Export** - Creates CSV file
**6 Zero Setup/Installation** - Runs entirely in Google Colab, no local installation needed

### Quick Start
Download .ipynb file, open it in Google Colab (https://colab.research.google.com/). You'll need to log into your Google account. 
1. Enter an Internet Archive collection ID (or item identifiers)
2. Run all cells in order (Runtime → Run all)
3. Review the generated reports
4. Download the CSV files at the end

### Requirements
requests>=2.28.0
pandas>=1.5.0
numpy>=1.23.0

### The Outputs
**Generated Files**
ia_metadata_snapshot.csv - Complete metadata export for all audited items
ia_metadata_issues_report.csv - Detailed issue log with severity, location, and notes

### Severity Levels
**HIGH**
missing_required - Critical fields (identifier, title) are empty
duplicate_identifier - Same identifier used multiple times
bad_date_format - Unparseable or invalid date values

**MEDIUM**
suspicious_year - Year outside range 1000–2100
mixed_value_types - Inconsistent data types across rows

 **LOW**
date_normalized - Valid timestamp that can be simplified
whitespace - Leading/trailing whitespace in fields


### License
This project is licensed under the MIT License - see the LICENSE file for details.

### Acknowledgments and Disclaimer
This tool has been built for the Internet Archive community and uses the Internet Archive APIs, but it it **NOT** an official project created and maintained by the Internet Archive.
It's inspired by real-world archival metadata challenges made by a user of the Internet Archive.
