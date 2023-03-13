# WAFeatureEnabler
Script that enables all AB features in WhatsApp Web.

## Usage
1. Open WhatsApp Web in your browser.
2. Press F12 to open the developer tools.
3. Go to the console tab.
4. Paste the script.
```js
const moduleRaid = function() {
    moduleRaid.mID = 'moduleRaid';
    moduleRaid.mObj = {};

    (window.webpackChunkwhatsapp_web_client).push([
        [moduleRaid.mID], {},
        function(e) {
            Object.keys(e.m).forEach(function(mod) {
                moduleRaid.mObj[mod] = e(mod);
            })
        }
    ])

    get = function get(id) {
        return moduleRaid.mObj[id];
    }

    findModule = function findModule(query) {
        results = [];
        modules = Object.keys(moduleRaid.mObj);

        modules.forEach(function(mKey) {
            mod = moduleRaid.mObj[mKey];

            if (!['function', 'string'].includes(typeof query)) {
                return;
            }
            if (typeof query === 'function' && query(mod)) {
                results.push(mod);
                return;
            }
            for (key in (mod?.default || mod)) {
                if (key == query) results.push(mod);
            }
        })

        return results;
    }

    return {
        modules: moduleRaid.mObj,
        constructors: moduleRaid.cArr,
        findModule: findModule,
        get: get
    }
}

if (typeof module === 'object' && module.exports) {
    module.exports = moduleRaid;
} else {
    window.mR = moduleRaid();
}
const moduleRaidInstance = new moduleRaid();
const features = {};
const getABPropConfigValue = moduleRaidInstance.findModule("getABPropConfigValue")[0].getABPropConfigValue;
console.log(`FeatureName${"\t".repeat(4)}Original Value${"\t".repeat(4)}new Value`);
moduleRaidInstance.findModule("getABPropConfigValue")[0].getABPropConfigValue = function(featureName) {
    const retVal = getABPropConfigValue(featureName);
    let newValue = retVal;
    if (typeof retVal === "boolean") {
        newValue = true;
    }
    if (features[featureName] === undefined) {
        console.log(`[+] ${featureName.padEnd(30)} ${retVal.toString().padEnd(30)} ${newValue.toString()}`);
    }
    features[featureName] = retVal;
    return newValue;
}
```
