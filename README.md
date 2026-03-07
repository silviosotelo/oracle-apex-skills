# Oracle APEX Skills for Claude Code

Public skills for Oracle APEX 20.2 development and Oracle Forms 6i migration, designed for use with [Claude Code](https://claude.com/claude-code).

## Skills

### oracle-apex-dev

Expert Oracle APEX 20.2 development assistant. Covers:
- Page, region, item, process, and dynamic action development
- JavaScript APIs (apex.server, apex.item, apex.region, apex.message, apex.theme)
- PL/SQL APIs (APEX_COLLECTION, APEX_UTIL, APEX_JSON, APEX_ESCAPE)
- Universal Theme styling and Inline Dialog patterns
- ORDS REST services
- Programmatic page generation with wwv_flow_api
- Interactive Grid and Interactive Report manipulation

### oracle-apex-migrate

Expert Oracle Forms 6i to APEX 20.2 migration assistant. Covers:
- Forms XML (Forms2XML) analysis
- Forms-to-APEX mapping rules (blocks, items, triggers, LOVs, security)
- Migration spec blueprints
- Programmatic APEX page generation from migration specs
- Forms trigger to APEX process/DA/validation mapping

## Target Environment

- **APEX:** 20.2.0.00.20
- **Database:** Oracle 12c R1.2+
- No features from APEX 21+ or Oracle 18c+

## References

Based on [Oracle APEX API 20.2](https://docs.oracle.com/en/database/oracle/application-express/20.2/aeapi/) official documentation.

## License

MIT
