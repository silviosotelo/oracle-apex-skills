# APEX 20.2 PL/SQL API Reference

## APEX_APPLICATION (Session State)

### Global Variables
| Variable | Description |
|----------|-------------|
| `g_x01` .. `g_x10` | Ajax callback parameters (from x01..x10) |
| `g_f01` .. `g_f20` | Array parameters (from f01..f20) |
| `g_clob_01` | CLOB parameter (from p_clob_01) |
| `g_user` | Current APEX user |
| `g_app_id` | Current application ID |
| `g_current_page_id` | Current page ID |
| `g_print_success_message` | Set page success message |

## APEX_COLLECTION

### Create
```plsql
apex_collection.create_collection('COLL_NAME');
apex_collection.create_or_truncate_collection('COLL_NAME');
apex_collection.create_collection_from_query_b(
    p_collection_name => 'COLL_NAME',
    p_query => 'SELECT col1, col2 FROM my_table WHERE ...'
);
```

### Members
```plsql
-- Add
apex_collection.add_member(
    p_collection_name => 'COLL_NAME',
    p_c001 => 'text_val',   -- up to c050
    p_n001 => 42,            -- up to n005
    p_d001 => SYSDATE        -- up to d005
);

-- Update (NEVER use UPDATE on apex_collections view!)
apex_collection.update_member(
    p_collection_name => 'COLL_NAME',
    p_seq => v_seq,
    p_c001 => 'new_val'
);

-- Delete
apex_collection.delete_member(p_collection_name => 'COLL_NAME', p_seq => v_seq);
apex_collection.delete_collection('COLL_NAME');

-- Count
v_count := apex_collection.get_member_count('COLL_NAME');
```

### Query
```sql
SELECT seq_id, c001, c002, n001, d001
FROM apex_collections
WHERE collection_name = 'COLL_NAME'
ORDER BY seq_id;
```

## APEX_JSON

### Write JSON Response
```plsql
apex_json.open_object;
apex_json.write('status', 'OK');
apex_json.write('count', v_count);
apex_json.open_array('items');
FOR rec IN (SELECT * FROM my_table) LOOP
    apex_json.open_object;
    apex_json.write('id', rec.id);
    apex_json.write('name', rec.name);
    apex_json.close_object;
END LOOP;
apex_json.close_array;
apex_json.close_object;
```

### Parse JSON
```plsql
apex_json.parse(p_source => v_json_clob);
v_status := apex_json.get_varchar2('status');
v_count  := apex_json.get_number('items[1].id');
```

## APEX_ESCAPE

```plsql
-- HTML escape (XSS prevention) - ALWAYS use for user input
v_safe := apex_escape.html(v_user_input);

-- HTML with whitespace preserved
v_safe := apex_escape.html_whitespace(v_text);

-- LDAP DN escape
v_safe := apex_escape.ldap_dn(v_dn);

-- LDAP search filter escape
v_safe := apex_escape.ldap_search_filter(v_filter);
```

## APEX_UTIL

```plsql
-- URL encode
v_encoded := apex_util.url_encode(v_param);

-- Get/set session state
v_val := apex_util.get_session_state('P10_NAME');
apex_util.set_session_state('P10_NAME', 'new_value');

-- Security group (workspace)
apex_util.set_security_group_id(v_workspace_id);

-- User management
v_exists := apex_util.is_login_password_valid(
    p_username => v_user, p_password => v_pass);
```

## APEX_MAIL

```plsql
apex_mail.send(
    p_to   => 'user@example.com',
    p_from => 'noreply@example.com',
    p_subj => 'Subject',
    p_body => 'Plain text body',
    p_body_html => '<h1>HTML body</h1>'
);
apex_mail.push_queue;  -- Actually send
```

## APEX_DEBUG

```plsql
apex_debug.enter('procedure_name');
apex_debug.message('Processing %s records', v_count);
apex_debug.warn('Unusual condition: %s', v_detail);
apex_debug.error('Error in %s: %s', $$plsql_unit, SQLERRM);
apex_debug.exit('procedure_name');
```

## APEX_WEB_SERVICE

```plsql
-- REST call
v_response := apex_web_service.make_rest_request(
    p_url         => 'https://api.example.com/data',
    p_http_method => 'POST',
    p_body        => v_json_body,
    p_credential_static_id => 'API_CREDS'  -- Shared Components > Web Credentials
);

-- Set headers
apex_web_service.g_request_headers.DELETE;
apex_web_service.g_request_headers(1).name := 'Content-Type';
apex_web_service.g_request_headers(1).value := 'application/json';

-- Check response
IF apex_web_service.g_status_code = 200 THEN
    -- parse response
END IF;
```

## APEX_THEME

```plsql
-- Get current theme ID
v_theme := apex_theme.get_theme_id;
```
