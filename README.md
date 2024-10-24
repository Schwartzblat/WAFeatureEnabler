# WAFeatureEnabler
Script that enables all AB features in WhatsApp Web.

## Usage
1. Open WhatsApp Web in your browser.
2. Press F12 to open the developer tools.
3. Go to the console tab.
4. Paste the script.
```js
const features = {};
const overrides = {
    web_intern_dogfooding_upsell_enabled: false,  // Causes a UI crash
};

const getABPropConfigValue = require('WAWebABProps').getABPropConfigValue;
console.log("FeatureName\t\t\t\tOriginal Value\t\t\t\tnew Value");
require('WAWebABProps').getABPropConfigValue = function (featureName) {
    const retVal = getABPropConfigValue(featureName);
    let newValue = retVal;
    if (typeof retVal === "boolean") {
        newValue = true;
    }
    if (features[featureName] === undefined) {
        console.log(`[+] ${featureName.padEnd(30)} ${retVal.toString().padEnd(30)} ${newValue.toString()}`);
    }
    features[featureName] = retVal;
    if (overrides[featureName] !== undefined) {
        return overrides[featureName];
    }
    return newValue;
}
```
