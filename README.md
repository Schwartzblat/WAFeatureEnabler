# WAFeatureEnabler
Script that enables all AB features in WhatsApp Web.

## Usage
1. Open WhatsApp Web in your browser.
2. Press F12 to open the developer tools.
3. Go to the console tab.
4. Paste the script.
```js
const moduleRaid = (() => {
  const mObj = {};
  (window.webpackChunkwhatsapp_web_client).push([
    ['moduleRaid'], {},
    (e) => {
      Object.keys(e.m).forEach((mod) => {
        mObj[mod] = e(mod);
      });
    }
  ]);

  const get = (id) => {
    return mObj[id];
  };

  const findModule = (query) => {
    const results = [];
    const modules = Object.keys(mObj);

    modules.forEach((mKey) => {
      const mod = mObj[mKey];

      if (typeof query !== 'function' && typeof query !== 'string') {
        return;
      }

      if (typeof query === 'function' && query(mod)) {
        results.push(mod);
        return;
      }

      for (const key in (mod?.default || mod)) {
        if (key === query) {
          results.push(mod);
        }
      }
    });

    return results;
  };

  return {
    modules: mObj,
    findModule: findModule,
    get: get
  };
})();

const features = {};
const getABPropConfigValue = moduleRaid.findModule("getABPropConfigValue")[0].getABPropConfigValue;
console.log("FeatureName\t\t\t\tOriginal Value\t\t\t\tNew Value");

moduleRaid.findModule("getABPropConfigValue")[0].getABPropConfigValue = function(featureName) {
  const retVal = getABPropConfigValue(featureName);
  let newValue = retVal;

  const featureList = ["edit", "bonsai", "community", "web", "username", "report", "receive", "wab", "view", "enabled" /*, "feature name" ...*/];
  if (featureList.some(element => featureName.includes(element))) {
    newValue = true;
  }

  if (features[featureName] === undefined) {
    console.log(`[+] ${featureName.padEnd(30)} ${retVal.toString().padEnd(30)} ${newValue.toString()}`);
  }

  features[featureName] = retVal;
  return newValue;
};
```
