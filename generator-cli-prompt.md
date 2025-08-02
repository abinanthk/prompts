# Enhanced CLI Documentation Generation Prompt

## Overview
Create a comprehensive CLI tool that generates API documentation spreadsheets and TypeScript code for Next.js projects. The generated files serve dual purposes:
1. **Code Generation**: Used by Node.js CLI to generate TypeScript models, services, and React Query hooks
2. **AI Integration**: Provides structured data for AI to understand APIs and generate frontend code bindings

## Core Requirements

### Tech Stack
- **Runtime**: Node.js with TypeScript
- **CLI Framework**: Commander.js for command structure
- **User Interaction**: Inquirer for interactive prompts
- **UI Feedback**: Ora for loading spinners, Chalk for colored output
- **Swagger Processing**: @apidevtools/swagger-parser (supports HTTP fetching)
- **File Operations**: fs-extra for enhanced file handling
- **Templating**: Handlebars for code template generation
- **Documentation**: xlsx for Excel spreadsheet generation
- **Configuration**: cosmiconfig for flexible config management
- **Validation**: Zod for schema validation
- **String Processing**: change-case for consistent naming conventions

### Package Configuration
- **Package Name**: `generator-cli`
- **CLI Command**: `generator-cli`
- **Naming Convention**: All files and folders in kebab-case

## Input Sources
- **Swagger Files**: Support for .yaml, .yml, and .json formats
- **Remote APIs**: HTTP/HTTPS URLs for fetching swagger configurations
- **Local Files**: File system paths to swagger documents

## Primary Output: Dual-Purpose Spreadsheets

### apis.xlsx Structure (AI-Optimized for Code Generation)

| Column | Purpose | AI Context | Example |
|--------|---------|------------|---------|
| **sNo** | Sequential numbering | Order of implementation | 1 |
| **endpoint** | API path | Route structure for frontend | /users/{id} |
| **method** | HTTP method | Operation type for UI logic | GET |
| **tag** | Swagger tag | Logical grouping for file organization | users |
| **operationId** | Unique operation identifier | Method and hook naming | getUserById |
| **summary** | Brief operation description | Component documentation | Get user by ID |
| **pathParams** | URL path parameters (JSON) | Dynamic routing requirements | `{"id":{"type":"string","required":true}}` |
| **queryParamsRef** | Reference to query params model | Filter and pagination logic | GetUserByIdQueryParams |
| **requestBodyRef** | Reference to request model | Input validation and forms | CreateUserInData |
| **responseBodyRef** | Reference to response model | Data display and state management | CreateUserOutData |
| **isPaginated** | Pagination indicator | List component requirements | true/false |
| **requiresAuth** | Authentication requirement | Route protection and auth state | true/false |
| **businessPurpose** | Functional context | AI understanding of feature purpose | User profile management for admin dashboard |

### models.xlsx Structure (Schema and Type Definitions)

| Column | Purpose | AI Context | Example |
|--------|---------|------------|---------|
| **sNo** | Sequential numbering | Generation order | 1 |
| **modelName** | Schema identifier | Type and interface naming | GetUsersParamsData |
| **properties** | Model properties (JSON) | Field structure and types | `{"id":"string","name":"string","email":"string"}` |
| **required** | Required fields (JSON array) | Validation and form requirements | `["id","name","email"]` |
| **description** | Model purpose | Business context for AI | User account entity with authentication data |
| **usedInOperations** | API usage mapping | Cross-reference for code generation | getUserById,updateUser,createUser |

## Generated Code Structure

### File Organization Pattern
```
src/
├── documents/
│   ├── apis.xlsx                       # Generated APIs document
│   └── models.xlsx                     # Generated Models document
├── constants/
│   ├── {tag}.constant.ts               # API endpoints and method constants
│   └── index.ts                        # Consolidated exports
├── models/
│   ├── {tag}/
│   │   ├── {operation-kebab}/
│   │   │   ├── {operation-kebab}-in-data.model.ts     # Request models
│   │   │   ├── {operation-kebab}-out-data.model.ts    # Response models
│   │   │   ├── {operation-kebab}-query-params.model.ts # Query parameters model
│   │   │   ├── {operation-kebab}-result-data.model.ts  # Nested result models (if needed)
│   │   │   ├── {operation-kebab}-record-data.model.ts  # Pagination record models (if needed)
│   │   │   └── index.ts                # Operation model exports
│   │   ├── {entity}-data.model.ts      # Base entity models
│   │   └── index.ts                    # Tag model exports
│   └── index.ts                        # All model exports
├── services/
│   ├── {tag}.service.ts                # Axios API services (one file per tag)
│   └── index.ts                        # All service exports
└── queries/
    ├── {tag}/
    │   ├── use-{operation-kebab}-query.query.ts    # Individual React Query hook files
    │   ├── use-{operation-kebab}-mutation.query.ts # Individual React Mutation hook files
    │   └── index.ts                    # Tag query exports
    └── index.ts                        # All query exports
```

## CLI Command Structure

### Document Generation Commands
```bash
# Generate API documentation spreadsheets from Swagger
generator-cli generate:documents --swagger-url <url> --output ./documents
generator-cli generate:documents --swagger-file ./swagger.yaml --output ./documents

# Validate Swagger files
generator-cli validate:swagger --swagger-file ./swagger.yaml

# Analyze generated spreadsheets
generator-cli analyze:docs --apis ./documents/apis.xlsx --models ./documents/models.xlsx
```

### Code Generation Commands
```bash
# Generate all code components
generator-cli generate:all --apis ./documents/apis.xlsx --models ./documents/models.xlsx

# Generate specific components
generator-cli generate:constants --apis ./documents/apis.xlsx --tag users
generator-cli generate:models --models ./documents/models.xlsx --tag users
generator-cli generate:services --apis ./documents/apis.xlsx --tag users
generator-cli generate:queries --apis ./documents/apis.xlsx --tag users

# Template management
generator-cli template:init --type constants
generator-cli template:update --type models --file ./custom-model.hbs
generator-cli template:list
```

### Configuration Commands
```bash
# Initialize configuration
generator-cli config:init --interactive

# Configuration management
generator-cli config:set outputDir ./src/generated
generator-cli config:set templateDir ./templates
generator-cli config:show
```

## AI Integration Features

### Context-Rich Documentation
The generated spreadsheets provide AI with comprehensive context:

1. **Business Understanding**: Purpose and domain context for each API
2. **Technical Specifications**: Complete schema and validation information
3. **Relationship Mapping**: Inter-model dependencies and associations
4. **UI Guidance**: Suggested components and user experience patterns
5. **Error Handling**: Expected error scenarios and user feedback requirements

### AI-Friendly Data Format
- **Structured JSON**: Complex data in parseable JSON format
- **Human-Readable Descriptions**: Clear business context and purpose
- **Cross-References**: Explicit relationships between models and operations
- **Validation Context**: Business rules and constraints for intelligent form generation
- **Performance Hints**: Caching and optimization suggestions

## Enhanced Use Cases

### For Code Generation CLI
- **Consistent Naming**: Automatic file and class name generation following conventions
- **Template-Driven**: Customizable Handlebars templates for different project patterns
- **Incremental Updates**: Support for updating existing code without conflicts
- **Validation**: Zod schemas for ensuring data integrity

### For AI Development Assistant
- **Smart Component Generation**: AI understands business context to suggest appropriate UI components
- **Intelligent Form Creation**: Auto-generate forms with proper validation and field types
- **State Management**: AI can recommend Redux/Zustand patterns based on data relationships
- **Error Handling**: Context-aware error boundaries and user feedback systems
- **Performance Optimization**: AI can suggest memoization and caching strategies

## Quality Assurance Features

### Validation and Verification
- **Schema Validation**: Ensure swagger compliance and completeness
- **Naming Consistency**: Verify kebab-case and naming convention adherence
- **Reference Integrity**: Validate model references and relationships
- **Business Logic**: Check for required fields and validation rules

### Documentation Quality
- **Completeness Checks**: Ensure all operations have adequate documentation
- **Business Context**: Verify meaningful descriptions and purpose statements
- **AI Readiness**: Validate that generated docs provide sufficient context for AI understanding

This enhanced structure ensures that the generated documentation serves as a comprehensive bridge between API specifications and both automated code generation and AI-assisted development, providing rich context for intelligent frontend development.