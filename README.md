##&lt;iron-localstorage&gt;

Element access to Web Session API (window.sessionStorage).

Keeps `value` property in sync with sessionStorage.

Value is saved as json by default.

###### Copyright (c) 2015 The PlatinumIndustries.pl. All rights reserved.

### Usage:

`ls-sample` will automatically save changes to its value.

```javascript
<dom-module id="ls-sample">
    <iron-sessionstorage
        name="my-app-storage"
        value="{{cartoon}}"
        on-iron-sessionstorage-load-empty="initializeDefaultCartoon"
    ></iron-sessionstorage>
</dom-module>

<script>
    Polymer({
        is: 'ls-sample',
        properties: {
            cartoon: {
                type: Object
            }
        },
        // initializes default if nothing has been stored
        initializeDefaultCartoon: function() {
            this.cartoon = {
                name: "Mickey",
                hasEars: true
            }
        },
        // use path set api to propagate changes to sessionstorage
        makeModifications: function() {
            this.set('cartoon.name', "Minions");
            this.set('cartoon.hasEars', false);
        }
    });
</script>
```

### Tech notes:

* * `value.*` is observed, and saved on modifications. You must use
    path change notifification methods such as `set()` to modify value
    for changes to be observed.

* * Set `auto-save-disabled` to prevent automatic saving.

* * Value is saved as JSON by default.

* * To delete a key, set value to null

* Element listens to StorageAPI `storage` event, and will reload upon receiving it.

* **Warning**: do not bind value to sub-properties until Polymer
[bug 1550](https://github.com/Polymer/polymer/issues/1550)
is resolved. session storage will be blown away.
`<iron-sessionstorage value="{{foo.bar}}"` will cause **data loss**.