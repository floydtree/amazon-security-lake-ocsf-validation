# OCSF Data Validation Tool

## About

A robust validation tool for ensuring your data conforms to the [Open Cybersecurity Schema Framework (OCSF)](https://schema.ocsf.io/) schemas. This tool helps security teams and developers validate their log data and event formats against the OCSF standard, which can be consumed in Amazon Security Lake.

_Note: If you are still generating data using the release candidate version of OCSF `v1.0.0-rc.2`, please use the older version of this tool, available [here]()._ 

### Features

- Supports JSON, gzipped JSON and Parquet file formats.
- Supports individual files or directories as input, locally or in S3.
- Provides 2 options for validation:
  - OCSF Server's validation API
  - JSON Schema Validation

### Official Resources
- [Amazon Security Lake Overview](https://aws.amazon.com/security-lake/)
- [Amazon Security Lake Custom Data](https://docs.aws.amazon.com/security-lake/latest/userguide/custom-sources.html)
- [OCSF Schema Browser](https://schema.ocsf.io/)
- [OCSF GitHub Org](https://github.com/ocsf)

## Getting Started

1. Clone the repository:

```bash
git clone [repository-url]
cd amazon-security-lake-ocsf-validation
```

2. Create and activate a virtual environment (recommended):

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows, use: .venv\Scripts\activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

## Usage

### Configuration

The first time you run the tool, you'll be guided through an interactive setup wizard to configure a few key items. These configuration settings are stored in `~/.ocsf_validator_config.json` and can be modified at any time. Key configuration options include:
- Directory to store validation failure results
- OCSF Server endpoint
- Option to include warnings encountered during validation
- Option to include full events that failed validation

Validate a single file or directory:

```bash
./ocsf_data_validator.py --input samples/bad-vpcflowlog-ocsf.json --auto-jsonschema --verbose
```

Enable verbose output for detailed validation information:

```bash
./ocsf_data_validator.py --input samples/ --auto-jsonschema --verbose
```

Validate data from S3:

```bash
./ocsf_data_validator.py --input s3://bucket-name/dir/file.parquet --auto-jsonschema
```

To reset to default configuration:

```bash
python ocsf_data_validator.py --reset-config
```

### Validation Results

The validator generates detailed error reports:

1. Console output (with `--verbose` flag)
2. Detailed validation errors in `failed_events.json`

#### Common Error types:

- Missing required fields
- Invalid data types
- Schema structure violations
- Unsupported attributes

#### Common Warning types:

- Expected regex mismatch
- Missing OCSF recommended fields (API validation only)

### Example Data

The `samples/` directory contains example OCSF-formatted data for testing:

- `good-vpcflowlog-ocsf.json`: Valid VPC flow log in OCSF format
- Additional samples demonstrating various event types and edge cases

### Schema Registry

The tool includes a schema registry containing jsonschema definitions for OCSF `v1.4.0`. These schemas are used to validate the structure and content of your data.

> Note: OCSF minor releases are backwards-compatible, meaning, we can use the latest stable version to validate data written using older minor versions in the `v1.0.0` release cycle.

## Contributing

Contributions are welcome! Please feel free to submit pull requests or create issues for bugs and feature requests. Refer to the [CONTRIBUTING](https://github.com/aws-samples/amazon-security-lake/blob/main/CONTRIBUTING.md) guide for details.
