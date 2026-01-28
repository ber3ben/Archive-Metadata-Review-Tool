# Archive Metadata Review Tool

A Python-based metadata quality audit tool for digital collections. Analyzes Internet Archive metadata to identify issues affecting discoverability and preservation, then generates prioritized reports for systematic review.

## What It Does

This tool helps surface quality issues in large metadata sets by:
- Validating metadata fields against configurable standards
- Detecting missing required fields, malformed dates, duplicate identifiers, and data type inconsistencies
- Categorizing issues by severity (HIGH/MEDIUM/LOW) so reviewers know where to focus
- Generating CSV reports that document what was found and where

**Important:** This tool identifies potential problems for human review. It doesn't automatically "fix" anything. The goal is to make metadata improvement more systematic and documented.

## Use Case

Designed for libraries, archives, and cultural heritage organizations dealing with metadata that has grown inconsistent over time due to:
- System migrations
- Staff changes
- Evolving cataloging standards
- Bulk imports from multiple sources

While this demonstration version works with Internet Archive's public API, the approach can be adapted to any metadata source.

## Features

- **Flexible Data Retrieval** – Fetches metadata via Internet Archive's advanced search API (by collection) or direct item endpoints (by identifier list)
- **Comprehensive Validation** – Checks for missing fields, date formatting issues, duplicate identifiers, whitespace problems, and mixed data types
- **Smart Date Handling** – Normalizes ISO 8601 timestamps (e.g., `2006-01-01T00:00:00Z` → `2006-01-01`) and validates date ranges
- **Severity Classification** – Issues categorized as HIGH, MEDIUM, or LOW priority based on impact
- **Transparent Reporting** – Every issue includes row number, field name, current value, and explanation
- **Zero Installation** – Runs entirely in Google Colab; no local setup required

## Quick Start

1. **Download** the `.ipynb` file from this repository
2. **Open** it in [Google Colab](https://colab.research.google.com/) (you'll need a Google account)
3. **Configure** the collection ID or item identifiers in the configuration cell
4. **Run** all cells in order (Runtime → Run all)
5. **Review** the generated reports in the notebook
6. **Download** the CSV files at the end for further analysis

### Example Configuration

```python
# For collection-based analysis:
MODE = "collection"
COLLECTION_ID = "internetarchivebooks"
MAX_ITEMS = 500

# For specific items:
MODE = "identifiers"
IDENTIFIERS = [
    "seeingwhatissacr0000gire",
    "walkingtourswash0000unse"
]
```

## Requirements

The tool uses standard Python libraries available in Google Colab:
- `requests>=2.28.0` – API calls
- `pandas>=1.5.0` – Data manipulation
- `numpy>=1.23.0` – Array operations

No manual installation needed when running in Colab.

## Output Files

### `ia_metadata_snapshot.csv`
Complete metadata export for all analyzed items. Use this to:
- See the full picture of your collection
- Filter and sort records by field
- Compare before/after cleanup

### `ia_metadata_issues_report.csv`
Prioritized issue log with columns:
- **severity** – HIGH/MEDIUM/LOW priority
- **row_number** – Approximate spreadsheet row (for cross-reference)
- **column** – Which metadata field has the issue
- **issue_type** – Specific problem category
- **value** – Current value (if applicable)
- **notes** – Explanation and suggested action

## Severity Levels

### HIGH Priority
Issues that significantly impact discoverability or indicate critical data problems:
- `missing_required` – Critical fields (identifier, title) are empty
- `duplicate_identifier` – Same identifier used multiple times
- `bad_date_format` – Unparseable or invalid date values

### MEDIUM Priority
Issues that affect consistency but don't break core functionality:
- `suspicious_year` – Year outside reasonable range (1000–2100)
- `mixed_value_types` – Inconsistent data types across rows (e.g., some records use strings, others use lists for the same field)

### LOW Priority
Cleanup opportunities that improve consistency:
- `date_normalized` – Valid timestamp that could be simplified (e.g., ISO 8601 → simple date)
- `whitespace` – Leading/trailing whitespace in fields

## Example Results

Running the tool on a 500-item sample of digitized books revealed:
- **500 total issues** across 10 metadata fields
- **497 date fields** with ISO timestamps that should be normalized
- **3 fields** with mixed data types (inconsistent structure)
- **0 HIGH-severity issues** (no missing identifiers or unparseable dates)

This prioritized list allowed reviewers to focus on structural consistency rather than emergency fixes.

## Customization

The tool is designed to be adapted. You can:
- Change `EXPECTED_FIELDS` to match your metadata schema
- Adjust `REQUIRED_IF_PRESENT` based on your cataloging requirements
- Modify date validation rules for historical collections
- Add new validation checks for domain-specific needs (e.g., controlled vocabularies)

## Limitations

- **Public data only** – This demonstration uses Internet Archive's public API; adapting it for internal systems requires API modifications
- **Sample size** – Advanced search returns a subset of fields; the full `/metadata/{identifier}` endpoint provides richer data
- **Date validation** – Lightly validated; some historical or non-standard date strings may be flagged incorrectly
- **No automatic fixes** – Intentionally does not modify source data; all corrections require human review

## Who This Is For

- **Metadata librarians** looking for systematic ways to improve catalog quality
- **Digital archivists** preparing collections for migration or publication
- **Collections managers** who need to understand the scope of cleanup work
- **Anyone** curious about metadata quality in large digital collections

## Technical Notes

The validation logic includes:
- Configurable field requirements
- ISO 8601 timestamp detection and normalization
- Basic date range validation (year, month, day)
- Type consistency checking across records
- Whitespace detection in text fields
- Duplicate identifier detection (with special handling for empty/null values)

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Acknowledgments and Disclaimer

This tool was built to demonstrate metadata quality workflows using the Internet Archive's public API. **It is not an official Internet Archive project** and is not affiliated with, endorsed by, or maintained by the Internet Archive.

The tool is inspired by real-world metadata challenges in libraries and archives. Thanks to the Internet Archive for maintaining a robust public API that makes projects like this possible.

## Contributing

This is a demonstration project, but suggestions and improvements are welcome. If you adapt this tool for your own collections or add useful validation checks, feel free to share your approach.

## Questions or Issues?

If you have questions about how the tool works or suggestions for improvement, please open an issue on this repository.

---

**Note:** This tool is meant to support human judgment, not replace it. Metadata quality requires understanding context, institutional standards, and user needs—things that code alone cannot determine. Use these reports as a starting point for informed decision-making.
