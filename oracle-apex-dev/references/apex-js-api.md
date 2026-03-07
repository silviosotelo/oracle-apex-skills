# APEX 20.2 JavaScript API Reference

## apex.navigation

### redirect
```javascript
apex.navigation.redirect('f?p=100:10:&APP_SESSION.');
```

### dialog.open / dialog.close
```javascript
// Open page as dialog
apex.navigation.dialog('f?p=100:20:&APP_SESSION.:::20:P20_ID:123',
    {title: 'Edit Record', height: 500, width: 700});

// Close dialog and pass values back
apex.navigation.dialog.close(true, {P20_RESULT: 'saved'});
```

## apex.item

### Core Methods
| Method | Description |
|--------|-------------|
| `getValue()` | Get current value |
| `setValue(val, suppressEvent)` | Set value, optionally suppress change event |
| `show()` / `hide()` | Toggle visibility |
| `enable()` / `disable()` | Toggle enabled state |
| `setFocus()` | Set keyboard focus |
| `isEmpty()` | Check if empty |
| `isChanged()` | Check if modified |
| `getValidity()` | Get validation state |
| `displayValueFor(val)` | Get display value for a return value |

### Shorthand
```javascript
$v('P10_NAME')              // getValue shorthand
$s('P10_NAME', 'new value') // setValue shorthand
```

## apex.region

### Core Methods
| Method | Description |
|--------|-------------|
| `refresh()` | Refresh region data |
| `focus()` | Focus region |

### Interactive Grid
```javascript
var ig = apex.region('empGrid').widget().interactiveGrid('getViews', 'grid');
var model = ig.model;
var record = model.getRecord(recordId);
model.setValue(record, 'SALARY', 5000);
ig.view$.grid('refresh');
```

### Interactive Report
```javascript
// Trigger action
apex.region('empReport').widget().interactiveReport('action_reset');
```

## apex.server

### process (Ajax Callback)
```javascript
apex.server.process('PROCESS_NAME', {
    x01: 'param1',           // up to x10
    x02: 'param2',
    f01: ['array', 'vals'],  // up to f20
    pageItems: '#P10_ID',    // comma-separated or jQuery selector
    p_clob_01: largeString   // CLOB parameter (apex_application.g_clob_01)
}, {
    dataType: 'json',        // or 'text', 'html'
    loadingIndicator: '#region_id',
    loadingIndicatorPosition: 'centered',
    success: function(data) { },
    error: function(jqXHR, textStatus, errorThrown) { }
});
```

### plugin (Execute plugin AJAX)
```javascript
apex.server.plugin(ajaxIdentifier, {
    pageItems: '#P10_ID'
}, {
    success: function(data) { }
});
```

## apex.message

| Method | Description |
|--------|-------------|
| `showPageSuccess(msg)` | Green success banner |
| `clearErrors()` | Clear all error messages |
| `showErrors(errArr)` | Show error messages |
| `confirm(msg, callback)` | Confirmation dialog |
| `alert(msg, callback)` | Alert dialog |

### Error array format
```javascript
apex.message.showErrors([
    { type: 'error', location: 'page', message: 'Page-level error' },
    { type: 'error', location: 'inline', pageItem: 'P10_NAME', message: 'Field error' }
]);
```

## apex.event

```javascript
// Bind to APEX events
apex.event.on(document, 'apexafterrefresh', function(e, data) {
    console.log('Region refreshed');
});

// Trigger custom event
apex.event.trigger('#P10_NAME', 'change');
```

## apex.util

| Method | Description |
|--------|-------------|
| `escapeHTML(str)` | Escape HTML entities (XSS prevention) |
| `showSpinner(container$)` | Show loading spinner, returns jQuery to remove |
| `delayLinger` | Manage spinner show/hide timing |

## apex.theme

| Method | Description |
|--------|-------------|
| `openRegion(staticId)` | Open Inline Dialog region |
| `closeRegion(staticId)` | Close Inline Dialog region |
