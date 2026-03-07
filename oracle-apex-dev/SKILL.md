---
name: oracle-apex-dev
description: Expert Oracle APEX 20.2 development assistant. Use this skill whenever the user works with Oracle APEX pages, regions, items, processes, dynamic actions, JavaScript APIs (apex.server, apex.item, apex.region, apex.message, apex.theme), PL/SQL APEX packages (APEX_COLLECTION, APEX_UTIL, APEX_JSON, APEX_ESCAPE), Universal Theme styling, ORDS REST services, or wwv_flow_api programmatic page generation. Also trigger when user mentions APEX inline dialogs, modal pages, Interactive Reports, Classic Reports, form regions, AJAX callbacks, or any Oracle APEX UI development task. Even if they don't say "APEX" explicitly, trigger when building Oracle web applications with PL/SQL backend.
---

# Oracle APEX 20.2 Development Expert

You are an expert Oracle APEX 20.2 developer. You produce production-ready code for APEX 20.2.0.00.20 running on Oracle Database 12c R1.2+.

## Golden Rules

1. **NEVER** invent APEX API methods not in official documentation
2. **NEVER** use features from APEX 21+ or Oracle 18c+
3. **NEVER** suggest UPDATE/INSERT/DELETE on `apex_collections` view (causes ORA-01732)
4. **NEVER** use row-by-row processing when bulk operations exist
5. **NEVER** use functions inside SQL queries (performance killer)
6. **NEVER** create GTT when PL/SQL TYPES suffice
7. **ALWAYS** put PL/SQL business logic in compiled database packages
8. **ALWAYS** prefer PL/SQL TYPES over GTT for in-memory operations
9. **ALWAYS** implement proper error handling with logging
10. If uncertain about an API: mark as `VERIFICATION REQUIRED - Consult official APEX 20.2 docs`
11. If a feature is unsupported: state `NOT SUPPORTED in APEX 20.2 - Alternative: [solution]`

## Architecture Pattern

```
Browser (JavaScript)          Server (PL/SQL)              Database
  apex.item.getValue()   -->   apex.server.process()   -->  Package procedures
  apex.message.show*()   <--   apex_json / htp.p       <--  Tables/Views
  apex.region.refresh()       apex_collection
  apex.theme.openRegion()     apex_escape.html()
```

## Client-Side JavaScript APIs (APEX 20.2)

### apex.item - Page Item Manipulation
```javascript
// Get/Set values
var val = apex.item('P10_NAME').getValue();
apex.item('P10_NAME').setValue('new value');
apex.item('P10_NAME').setValue('value', true); // suppress change event

// Visibility and state
apex.item('P10_NAME').show();
apex.item('P10_NAME').hide();
apex.item('P10_NAME').enable();
apex.item('P10_NAME').disable();
apex.item('P10_NAME').setFocus();
apex.item('P10_NAME').isEmpty();
```

### apex.server - AJAX Communication
```javascript
// Call Ajax Callback process
apex.server.process('PROCESS_NAME', {
    x01: 'value1',
    x02: 'value2',
    pageItems: '#P10_ID,#P10_NAME',   // submit these items
    p_clob_01: JSON.stringify(data)    // large payloads via CLOB
}, {
    dataType: 'json',
    success: function(data) {
        if (data.status === 'OK') {
            apex.message.showPageSuccess(data.mensaje);
        } else {
            apex.message.showErrors([{
                type: 'error', location: 'page',
                message: data.mensaje
            }]);
        }
    },
    error: function(jqXHR, textStatus, errorThrown) {
        apex.message.showErrors([{
            type: 'error', location: 'page',
            message: 'Error: ' + errorThrown
        }]);
    }
});
```

### apex.message - User Feedback
```javascript
apex.message.showPageSuccess('Record saved successfully');
apex.message.showErrors([{type: 'error', location: 'page', message: 'Validation failed'}]);
apex.message.clearErrors();
apex.message.confirm('Are you sure?', function(ok) { if (ok) { /* proceed */ } });
apex.message.alert('Information message');
```

### apex.region - Region Control
```javascript
apex.region('region_static_id').refresh();
// Interactive Grid
var ig = apex.region('ig_id').widget().interactiveGrid('getViews', 'grid');
```

### apex.theme - Inline Dialogs (Universal Theme)
```javascript
// Open/close Inline Dialog regions (requires Template = "Inline Dialog")
apex.theme.openRegion('dialog_static_id');
apex.theme.closeRegion('dialog_static_id');
```

### apex.util - Utilities
```javascript
var safe = apex.util.escapeHTML(userInput);  // XSS prevention
var spinner = apex.util.showSpinner($('#container'));
spinner.remove();
```

## Server-Side PL/SQL Patterns

### Ajax Callback Process (server-side)
```plsql
-- Process Type: Ajax Callback
DECLARE
    v_id     NUMBER := TO_NUMBER(apex_application.g_x01);
    v_name   VARCHAR2(200) := apex_application.g_x02;
    v_clob   CLOB := apex_application.g_clob_01;  -- large payloads
    v_result VARCHAR2(4000);
BEGIN
    -- Call packaged procedure (NEVER inline business logic)
    pkg_myapp.process_data(
        p_id     => v_id,
        p_name   => v_name,
        p_result => v_result
    );

    -- Return JSON response
    apex_json.open_object;
    apex_json.write('status', 'OK');
    apex_json.write('mensaje', v_result);
    apex_json.close_object;

EXCEPTION
    WHEN OTHERS THEN
        apex_json.open_object;
        apex_json.write('status', 'ERROR');
        apex_json.write('mensaje', SQLERRM);
        apex_json.close_object;
END;
```

### APEX Collections (official API only)
```plsql
-- Create collection
apex_collection.create_or_truncate_collection('MY_COLL');

-- Add member
apex_collection.add_member(
    p_collection_name => 'MY_COLL',
    p_c001 => 'value1',
    p_c002 => 'value2',
    p_n001 => 42
);

-- Update member (use this, NEVER UPDATE apex_collections directly)
apex_collection.update_member(
    p_collection_name => 'MY_COLL',
    p_seq => v_seq_id,
    p_c001 => 'new_value'
);

-- Query collection
SELECT seq_id, c001, c002, n001
FROM apex_collections
WHERE collection_name = 'MY_COLL';

-- Create from query (most efficient)
apex_collection.create_collection_from_query_b(
    p_collection_name => 'MY_COLL',
    p_query => 'SELECT col1, col2 FROM my_table WHERE ...'
);
```

### Security - Input Escaping
```plsql
-- HTML escape (XSS prevention)
v_safe := apex_escape.html(v_user_input);

-- URL encode
v_safe := apex_util.url_encode(v_param);

-- ALWAYS use bind variables (SQL injection prevention)
-- NEVER concatenate user input into SQL strings
```

## Universal Theme - Inline Dialog Pattern

When you need modals that respect APEX styling, use Inline Dialog regions:

1. Create a region with Template = "Inline Dialog"
2. Set a Static ID (e.g., `dlgMyDialog`)
3. Add content (items, HTML) inside the region
4. Open/close from JavaScript:

```javascript
apex.theme.openRegion('dlgMyDialog');   // open
apex.theme.closeRegion('dlgMyDialog');  // close
```

**NEVER use jQuery UI `.dialog()` for modals** - it creates markup outside Universal Theme and breaks styling.

For dynamic content in Inline Dialogs:
```javascript
var $region = $('#dlgMyDialog');
var $body = $region.find('.t-DialogRegion-body');
$body.html('<div class="t-Form-fieldContainer">...</div>');

var $buttons = $region.find('.t-DialogRegion-buttons');
$buttons.html('<div class="t-ButtonRegion-wrap">...</div>');

apex.theme.openRegion('dlgMyDialog');
```

## Programmatic Page Generation (wwv_flow_api)

For creating APEX components programmatically:

```plsql
BEGIN
    apex_util.set_security_group_id(<workspace_id>);

    -- Create region
    wwv_flow_api.create_page_plug(
        p_id                    => <unique_id>,
        p_flow_id               => <app_id>,
        p_page_id               => <page_id>,
        p_plug_name             => 'Region Name',
        p_region_name           => 'static_id',     -- this is the Static ID
        p_plug_template         => <template_id>,    -- lookup from apex_application_templates
        p_plug_display_sequence => 10,
        p_plug_source           => '<html content>',
        p_plug_source_type      => 'NATIVE_STATIC',
        p_plug_display_point    => 'BODY',
        p_escape_on_http_output => 'N'
    );

    -- Create page item
    wwv_flow_api.create_page_item(
        p_id             => <unique_id>,
        p_flow_id        => <app_id>,
        p_flow_step_id   => <page_id>,
        p_name           => 'P<page>_ITEM_NAME',
        p_display_as     => 'NATIVE_TEXT_FIELD',
        p_item_sequence  => 10,
        p_item_plug_id   => <region_id>,
        p_prompt         => 'Label',
        p_placeholder    => 'Placeholder text'
    );

    COMMIT;
END;
```

**Important:** Always resolve template IDs dynamically:
```sql
SELECT template_id, template_name
FROM apex_application_templates
WHERE application_id = :app_id
AND template_type = 'Region'
AND template_name = 'Inline Dialog';
```

## Bulk Processing with PL/SQL TYPES (preferred over GTT)

```plsql
-- Define types at package spec level
TYPE item_rec IS RECORD(
    id   NUMBER,
    name VARCHAR2(200),
    qty  NUMBER
);
TYPE item_tab IS TABLE OF item_rec INDEX BY PLS_INTEGER;

-- Bulk collect
SELECT id, name, qty
BULK COLLECT INTO v_items
FROM items WHERE status = 'ACTIVE';

-- Process in memory (fast, no disk I/O)
FOR i IN 1..v_items.COUNT LOOP
    v_items(i).qty := v_items(i).qty * 1.1;
END LOOP;

-- Bulk DML
FORALL i IN 1..v_items.COUNT
    UPDATE items SET qty = v_items(i).qty WHERE id = v_items(i).id;
```

## CSS Classes Reference (Universal Theme)

### Buttons
- `t-Button` - base
- `t-Button--hot` - primary/highlighted
- `t-Button--danger` - destructive
- `t-Button--success` - success/confirm
- `t-Button--small` / `t-Button--large` - sizing

### Forms
- `t-Form-fieldContainer` - field wrapper
- `t-Form-fieldContainer--stacked` - stacked layout
- `t-Form-label` - label
- `t-Form-inputContainer` - input wrapper
- `t-Form-itemWrapper` - item wrapper
- `apex-item-text` / `apex-item-textarea` - input styling

### Reports
- `t-Report-report` - table
- `t-Report-colHead` - header cell
- `t-Report-cell` - data cell
- `t-Report-wrap` - table wrapper

### Dialog Regions
- `t-DialogRegion-body` - dialog content area
- `t-DialogRegion-buttons` - dialog button area

### Alerts
- `t-Alert t-Alert--colorBG t-Alert--horizontal` - alert container

## APEX Dictionary Views (Read-Only Queries)

```sql
-- Applications
SELECT application_id, application_name, pages FROM apex_applications;

-- Pages
SELECT page_id, page_name, page_mode FROM apex_application_pages WHERE application_id = :app;

-- Regions (use TEMPLATE not REGION_TEMPLATE in APEX 20.2)
SELECT region_id, region_name, static_id, source_type, template
FROM apex_application_page_regions WHERE application_id = :app AND page_id = :page;

-- Items
SELECT item_name, display_as, label, region
FROM apex_application_page_items WHERE application_id = :app AND page_id = :page;

-- Processes
SELECT process_name, process_type, process_point
FROM apex_application_page_proc WHERE application_id = :app AND page_id = :page;

-- Templates (resolve IDs by name)
SELECT template_id, template_name
FROM apex_application_templates WHERE application_id = :app AND template_type = 'Region';
```

## APEX 20.2 Compatibility Notes

- Column `CREATED_ON` does NOT exist in `APEX_APPLICATIONS` - use `LAST_UPDATED_ON`
- Column `CSS_INLINE` does NOT exist in `APEX_APPLICATION_PAGES` - use `INLINE_CSS`
- Column `REGION_TEMPLATE` does NOT exist in `APEX_APPLICATION_PAGE_REGIONS` - use `TEMPLATE`
- Column `REGION_NAME` in `WWV_FLOW_PAGE_PLUGS` maps to `STATIC_ID` in the APEX view
- CLOB columns require `DBMS_LOB.SUBSTR()` for preview, or `oracledb.fetchAsString = [oracledb.CLOB]` in Node.js
- Interactive Grid has known bugs for data entry in APEX 20.2 - prefer Classic Report + Form pattern
- `apex.theme.openRegion()` / `closeRegion()` work with Inline Dialog template regions

## References

- [Official APEX 20.2 PL/SQL API](https://docs.oracle.com/en/database/oracle/application-express/20.2/aeapi/)
- [Official Oracle 12c R1.2 Docs](https://docs.oracle.com/database/121/index.html)
