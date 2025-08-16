# IXCOACH API FILE LOCATION MAP - PRECISE RULES FOR AI AGENTS

## EXACT DIRECTORY PATHS AND CONTENT PLACEMENT RULES

| File Type | EXACT Directory Path | No Exceptions |
|-----------|---------------------|---------------|
| **MODULE DIRECTORIES** | `/ixcoach-api/{module-name}/` | Self-contained module with clear boundary |
| **MODULE README** | `/ixcoach-api/{module-name}/README.md` | Module overview, usage guide, and architecture |
| **MODULE CODE** | `/ixcoach-api/{module-name}/index.js` | Public API and entry points only |
| **MODULE DOCUMENTATION** | `/ixcoach-api/{module-name}/docs/` | Module-specific documentation |
| **MODULE ISSUES** | `/ixcoach-api/{module-name}/docs/issues/` | Module-specific issue tracking |
| **ENTITY RESOLVERS** | `/ixcoach-api/{module-name}/entity-resolvers/` | Data retrieval without modification |
| **EVENT HANDLERS** | `/ixcoach-api/{module-name}/event-handlers/` | Event processing logic |
| **PERSISTENCE LAYER** | `/ixcoach-api/{module-name}/persistence/` | Database operations only |
| **STATE DETERMINERS** | `/ixcoach-api/{module-name}/state-determiners/` | Pure decision functions |
| **VALIDATORS** | `/ixcoach-api/{module-name}/validators/` | Input validation only |
| **SIDE EFFECTS** | `/ixcoach-api/{module-name}/side-effects/` | Notifications, logging, external calls |
| **UTILS** | `/ixcoach-api/{module-name}/utils/` | Helper utilities |
| **MODULE MOCKS** | `/ixcoach-api/{module-name}/mocks/` | Test data and mocks |
| **MODULE TESTS** | `/ixcoach-api/{module-name}/tests/` | Module-specific tests |
| **MODULE UNIT TESTS** | `/ixcoach-api/{module-name}/tests/unit/` | Unit test files |
| **MODULE INTEGRATION** | `/ixcoach-api/{module-name}/integration/` | Integration with other modules |
| **COMPONENT TESTS** | `/ixcoach-api/{module-name}/{component}/\_\_tests\_\_/` | Component-specific tests |

## STANDARD MODULE STRUCTURE

```
/module-name/
├── 🟢 README.md                    # Module overview, usage guide, and architecture
├── 🟢 index.js                     # Public API exports
├── 🟢 docs/                        # Module-specific documentation
│   ├── 🟢 README.md                # Documentation overview
│   ├── 🟢 COMPONENT_GUIDE.md       # Component usage guide
│   ├── 🟢 changelog/               # Module-specific changelog
│   └── 🟢 issues/                  # Module-specific issues
├── 🟢 entity-resolvers/            # Data retrieval without modification
│   ├── 🟢 README.md                # Entity resolver documentation
│   ├── 🟢 entity-resolver.js       # Entity resolver implementation
│   └── 🟢 __tests__/               # Entity resolver tests
├── 🟢 event-handlers/              # Event processing logic
│   ├── 🟢 event-handler.js         # Event handler implementation
│   └── 🟢 __tests__/               # Event handler tests
├── 🟢 persistence/                 # Database operations only
│   ├── 🟢 entity-persistence.js    # Entity persistence implementation
│   └── 🟢 __tests__/               # Persistence tests
├── 🟢 state-determiners/           # Pure decision functions
│   └── 🟢 state-determiner.js      # State determiner implementation
├── 🟢 validators/                  # Input validation only
│   └── 🟢 input-validator.js       # Validator implementation
├── 🟢 side-effects/                # Notifications, logging, external calls
│   └── 🟢 notification-sender.js   # Side effect implementation
├── 🟢 utils/                       # Helper utilities
│   └── 🟢 helper.js                # Utility implementation
├── 🟢 mocks/                       # Test data and mocks
│   ├── 🟢 README.md                # Mock documentation
│   └── 🟢 CONFIRMED_event.json     # Verified test data
├── 🟢 tests/                       # Module-specific tests
│   ├── 🟢 README.md                # Test documentation
│   ├── 🟢 unit/                    # Unit tests
│   └── 🟢 integration/             # Integration tests
└── 🟢 integration/                 # Integration with other modules
    └── 🟢 module-adapter.js        # Adapter implementation
```

## NON-COMPLIANT STRUCTURES TO AVOID

```
/ixcoach-api/
├── 🟡 __tests__/                   # AVOID: Root level test directory
├── 🟡 tests/                       # AVOID: Root level tests directory
├── 🟡 test/                        # AVOID: Legacy test directory
├── 🟡 controllers/                 # AVOID: Flat controller directory
├── 🟡 routes/                      # AVOID: Flat routes directory
├── 🟡 models/                      # AVOID: Flat models directory
├── 🟡 middlewares/                 # AVOID: Flat middleware directory
├── 🟡 utils/                       # AVOID: Global utils directory
└── 🟡 docs/                        # AVOID: Global docs directory
```

## CRITICAL MESSAGE TO ALL AI AGENTS

All new code and documentation MUST follow the modular structure shown above with the green dots. The non-compliant structure with yellow dots represents legacy organization that should NOT be expanded.

When creating new features or refactoring existing ones:

1. Create a self-contained module with clear boundaries
2. Follow the layered architecture pattern shown above
3. Include tests alongside the components they test
4. Document the module in its own docs directory
5. Export a clean public API through the module's index.js

Do NOT place new code in the global directories marked with yellow dots. Instead, create a properly structured module that follows the pattern shown in the green section.