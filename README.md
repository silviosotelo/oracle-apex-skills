# Oracle APEX Skills for Claude Code

Public skills for Oracle APEX development and Oracle Forms migration, designed for use with [Claude Code](https://claude.com/claude-code).

**Supports ALL Oracle APEX versions** from 19.2 through 24.2, with automatic adaptation to the user's specific APEX and Oracle DB version.

## Skills

### oracle-apex-dev

Expert Oracle APEX development assistant. Version-aware — adapts to the user's APEX version automatically. Covers:
- Page, region, item, process, and dynamic action development
- JavaScript APIs (apex.server, apex.item, apex.region, apex.message, apex.theme, apex.date, apex.pwa)
- PL/SQL APIs (APEX_COLLECTION, APEX_UTIL, APEX_JSON, APEX_ESCAPE, APEX_AI, APEX_PWA, etc.)
- Universal Theme styling and Inline Dialog patterns
- ORDS REST services
- Programmatic page generation with wwv_flow_api
- Interactive Grid and Interactive Report manipulation
- Version compatibility matrix for all APIs and features

### oracle-apex-migrate

Expert Oracle Forms 6i to APEX migration assistant. Supports migration to any APEX version. Covers:
- Forms XML (Forms2XML) analysis
- Forms-to-APEX mapping rules (blocks, items, triggers, LOVs, security)
- Migration spec blueprints adapted to target APEX version
- Programmatic APEX page generation from migration specs
- Forms trigger to APEX process/DA/validation mapping
- Version-specific feature recommendations

## Supported Versions

| APEX | Min DB | Key Features |
|---|---|---|
| 19.1 - 19.2 | 11.2+ | Base APIs |
| 20.1 - 20.2 | 11.2+ | +APEX_EXEC, +Faceted Search |
| 21.1 - 21.2 | 12.1+ | +Friendly URLs, +Cards, +apex.date |
| 22.1 - 22.2 | 19c+ | +Approvals, +Map regions |
| 23.1 - 23.2 | 19c+ | +Workflow, +PWA, +APEX_BARCODE |
| 24.1 - 24.2 | 19c+ | +APEX_AI, +AI Assistant |

## References

Based on official Oracle APEX documentation for all versions:
- [APEX 24.2 Docs](https://docs.oracle.com/en/database/oracle/apex/24.2/)
- [APEX 20.2 Docs](https://docs.oracle.com/en/database/oracle/application-express/20.2/)
- [All versions archive](https://www.oracle.com/database/technologies/appdev/apex_doc_archives.html)

## License

MIT
