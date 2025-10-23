# This folder will contain the extracted APIM configurations

The `artifacts` folder is where the backup pipeline will store all extracted Azure API Management configurations, including:

- APIs and their specifications
- Products and subscriptions
- Policies (service, API, and operation level)
- Named values and backends
- Diagnostics and loggers
- Version sets and workspaces
- And other APIM resources

## Structure

When the extractor runs, it will create a folder structure similar to:
```
artifacts/
├── apis/
│   ├── api-name/
│   │   ├── information.json
│   │   ├── specification.yaml
│   │   └── policies/
├── products/
├── namedValues/
├── backends/
├── policies/
└── ... (other resources)
```

## Usage

- The backup pipeline writes to this folder
- The restore pipeline reads from this folder
- Version control tracks changes to these files
- Manual edits can be made before restore if needed