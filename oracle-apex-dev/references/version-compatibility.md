# Oracle APEX Version Compatibility Reference

## APEX Version Timeline & DB Requirements

| APEX | Release Date | Min DB | URL Format | Theme |
|---|---|---|---|---|
| 19.1 | 2019-Q1 | 11.2 | Legacy (f?p=) | Universal Theme 42 |
| 19.2 | 2019-Q4 | 11.2 | Legacy | UT 42 |
| 20.1 | 2020-Q2 | 11.2 | Legacy | UT 42 |
| 20.2 | 2020-Q4 | 11.2 | Legacy | UT 42 |
| 21.1 | 2021-Q2 | 12.1 | Friendly URLs | UT 42 |
| 21.2 | 2021-Q4 | 12.1 | Friendly URLs | UT 42 |
| 22.1 | 2022-Q2 | 19c | Friendly URLs | UT 42 |
| 22.2 | 2022-Q4 | 19c | Friendly URLs | UT 42 |
| 23.1 | 2023-Q2 | 19c | Friendly URLs | UT 42 |
| 23.2 | 2023-Q4 | 19c | Friendly URLs | UT 42 |
| 24.1 | 2024-Q2 | 19c | Friendly URLs | UT 42 |
| 24.2 | 2024-Q4 | 19c | Friendly URLs | UT 42 |

## PL/SQL Package Availability Matrix

### Core Packages (ALL versions 19.1+)
| Package | Description |
|---|---|
| APEX_APPLICATION | Session state, global vars (g_x01-x10, g_f01-f20, g_clob_01) |
| APEX_COLLECTION | Temporary session collections (create, add_member, update_member) |
| APEX_DEBUG | Debug logging (enter, message, warn, error, exit) |
| APEX_ESCAPE | XSS prevention (html, html_whitespace, ldap_dn) |
| APEX_ERROR | Error handling framework |
| APEX_EXPORT | Application export |
| APEX_IG | Interactive Grid operations |
| APEX_IR | Interactive Report operations |
| APEX_ITEM | Item rendering (legacy, use page items instead) |
| APEX_JSON | JSON write/parse |
| APEX_LANG | Translations and message lookup |
| APEX_LDAP | LDAP integration |
| APEX_MAIL | Email sending (send, push_queue) |
| APEX_PAGE | Page operations |
| APEX_PLUGIN | Plugin framework |
| APEX_REGION | Region operations |
| APEX_SESSION | Session management |
| APEX_STRING | String utilities (split, join, format, push) |
| APEX_THEME | Theme operations |
| APEX_UTIL | General utilities (get/set_session_state, url_encode) |
| APEX_ACL | Access Control Lists |
| APEX_APP_SETTING | Application settings |
| APEX_AUTHENTICATION | Authentication schemes |
| APEX_AUTHORIZATION | Authorization schemes |
| APEX_CREDENTIAL | Web credentials management |
| APEX_CSS | CSS utilities |
| APEX_JAVASCRIPT | JavaScript utilities |
| APEX_INSTANCE_ADMIN | Instance administration |

### 20.1+ Packages
| Package | Description |
|---|---|
| APEX_EXEC | Execute queries against local/remote/REST sources |
| APEX_DATA_PARSER | Parse CSV, JSON, XML, XLSX files |
| APEX_DATA_EXPORT | Export data in multiple formats |
| APEX_JWT | JSON Web Token creation/validation |
| APEX_AUTOMATION | Scheduled automated tasks |
| APEX_REST_SOURCE_SYNC | REST Data Source synchronization |
| APEX_SPATIAL | Spatial/map operations |

### 21.1+ Packages
| Package | Description |
|---|---|
| APEX_MARKDOWN | Markdown to HTML conversion |
| APEX_DATA_LOADING | Data loading utilities |

### 22.1+ Packages
| Package | Description |
|---|---|
| APEX_APPROVAL | Approval workflow management |
| APEX_DG_DATA_GEN | Data Generator (blueprints, test data) |
| APEX_SEARCH | Unified Search configuration |
| APEX_SESSION_STATE | Enhanced session state management |

### 23.1+ Packages
| Package | Description |
|---|---|
| APEX_BACKGROUND_PROCESS | Background PL/SQL execution |
| APEX_BARCODE | QR/barcode generation |
| APEX_HUMAN_TASK | Human task management (workflows) |
| APEX_PWA | Progressive Web App features |

### 24.1+ Packages
| Package | Description |
|---|---|
| APEX_AI | Generative AI / LLM integration |
| APEX_HTTP | HTTP client (modern replacement for APEX_WEB_SERVICE patterns) |
| APEX_EXTENSION | Extension framework |
| APEX_APPLICATION_ADMIN | Application admin operations |

## JavaScript API Availability Matrix

### Core (ALL versions 19.1+)
- `apex.item` — getValue, setValue, show, hide, enable, disable, setFocus, isEmpty
- `apex.region` — refresh, focus, widget()
- `apex.server` — process (AJAX callbacks), plugin
- `apex.message` — showPageSuccess, showErrors, clearErrors, confirm, alert
- `apex.page` — submit, validate, cancelWarnOnUnsavedChanges
- `apex.theme` — openRegion, closeRegion (Inline Dialogs)
- `apex.util` — escapeHTML, showSpinner, delayLinger
- `apex.navigation` — redirect, dialog.open, dialog.close
- `apex.event` — on, trigger
- `apex.actions` — action framework
- `apex.debug` — log, info, warn, error
- `apex.lang` — getMessage, formatMessage
- `apex.locale` — getLanguage, getGroupSeparator, getDecimalSeparator
- `apex.model` — data model operations
- `apex.storage` — session/local storage wrapper
- `apex.da` — dynamic action utilities
- `apex.widget` — widget utilities

### 21.1+ (Friendly URLs era)
- `apex.date` — parse, format, add, subtract, isValid, UNIT constants

### 23.1+ (PWA era)
- `apex.pwa` — installApp, isPWAInstalled

### 24.1+ (AI era)
- New interfaces: `cardsRegion`, `facetsRegion`, `mapRegion`, `templateReportRegion`

## Region Types by Version

| Region Type | Available From |
|---|---|
| Static Content | All |
| Classic Report | All |
| Interactive Report | All |
| PL/SQL Dynamic Content | All |
| URL | All |
| List | All |
| Breadcrumb | All |
| Interactive Grid | 5.1+ |
| Form | 19.1+ (native) |
| Faceted Search | 20.1+ |
| Smart Filters | 20.1+ |
| Cards | 21.1+ |
| Content Row | 21.1+ |
| Map | 21.2+ |
| Workflow | 23.1+ |
| Approvals | 23.1+ |
| AI Assistant | 24.1+ |

## APEX Dictionary View Column Differences

### APEX_APPLICATIONS
| Column | Versions | Notes |
|---|---|---|
| APPLICATION_ID | All | |
| APPLICATION_NAME | All | |
| ALIAS | All | |
| OWNER | All | |
| WORKSPACE | All | |
| PAGES | All | Page count (direct column) |
| LAST_UPDATED_ON | All | |
| CREATED_ON | 21.1+ | Does NOT exist in 19.x/20.x |
| AUTHENTICATION_SCHEME | All | |
| COMPATIBILITY_MODE | All | |

### APEX_APPLICATION_PAGES
| Column | Versions | Notes |
|---|---|---|
| PAGE_ID | All | |
| PAGE_NAME | All | |
| PAGE_MODE | All | NORMAL, MODAL_DIALOG, NON_MODAL_DIALOG |
| PAGE_GROUP | All | |
| PAGE_TEMPLATE | All | |
| INLINE_CSS | 20.1+ | Was CSS_INLINE in pre-20.1 |
| JAVASCRIPT_CODE | All | CLOB |
| PAGE_CSS_CLASSES | 21.1+ | |
| LAST_UPDATED_ON | All | |

### APEX_APPLICATION_PAGE_REGIONS
| Column | Versions | Notes |
|---|---|---|
| REGION_ID | All | |
| REGION_NAME | All | Display name |
| STATIC_ID | All | HTML id attribute |
| SOURCE_TYPE | All | |
| TEMPLATE | 20.1+ | Was REGION_TEMPLATE in pre-20.1 |
| DISPLAY_SEQUENCE | All | |
| DISPLAY_POSITION | All | |
| REGION_SOURCE | All | CLOB |
| CONDITION_TYPE | All | |

### Internal Table Mappings
| APEX View | Internal Table | Notes |
|---|---|---|
| STATIC_ID | WWV_FLOW_PAGE_PLUGS.REGION_NAME | NOT region_attributes_substitution |
| TEMPLATE | WWV_FLOW_PAGE_PLUGS.PLUG_TEMPLATE | Template ID number |
| REGION_NAME | WWV_FLOW_PAGE_PLUGS.PLUG_NAME | Display name |

## Oracle DB PL/SQL Features by Version

| Feature | Min Version |
|---|---|
| BULK COLLECT / FORALL | 8i+ |
| Associative arrays (INDEX BY) | 9i+ |
| FETCH FIRST N ROWS ONLY | 12c (12.1) |
| Identity columns | 12c (12.1) |
| PL/SQL in WITH clause | 12c (12.1) |
| ACCESSIBLE BY clause | 12c (12.1) |
| JSON support (IS JSON, JSON_VALUE) | 12c (12.1) |
| JSON_TABLE, JSON_OBJECT | 12c R2 (12.2) |
| Qualified expressions (PL/SQL) | 18c |
| Polymorphic table functions | 18c |
| LISTAGG with DISTINCT | 19c |
| SQL macros | 21c |
| JSON Relational Duality | 23ai |
| Boolean in SQL | 23ai |
| IF [NOT] EXISTS for DDL | 23ai |
| SQL Domains | 23ai |

## Password Verifier Compatibility (oracledb driver)

| DB Version | Verifier | oracledb Thin Mode | oracledb Thick Mode |
|---|---|---|---|
| 11g | 10G (0x939) | NOT supported | Supported |
| 12c+ | 12C (SHA-512) | Supported | Supported |
| 18c+ | Default 12C | Supported | Supported |

If connecting to 11g/12c with old password verifiers, use Thick mode with Oracle Instant Client.
