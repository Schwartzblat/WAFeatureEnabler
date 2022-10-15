# WAFeatureEnabler
Script that enables all AB features in WhatsApp Web.

## Usage
1. Open WhatsApp Web in your browser.
2. Press F12 to open the developer tools.
3. Go to the console tab.
4. Paste the script.
```js
const moduleRaid = function () {
  moduleRaid.mID  = Math.random().toString(36).substring(7);
  moduleRaid.mObj = {};

  fillModuleArray = function() {
    (window.webpackChunkbuild || window.webpackChunkwhatsapp_web_client).push([
      [moduleRaid.mID], {}, function(e) {
        Object.keys(e.m).forEach(function(mod) {
          moduleRaid.mObj[mod] = e(mod);
        })
      }
    ]);
  }

  fillModuleArray();

  get = function get (id) {
    return moduleRaid.mObj[id]
  }

  findModule = function findModule (query) {
    results = [];
    modules = Object.keys(moduleRaid.mObj);

    modules.forEach(function(mKey) {
      mod = moduleRaid.mObj[mKey];

      if (typeof mod !== 'undefined') {
        if (typeof query === 'string') {
          if (typeof mod.default === 'object') {
            for (key in mod.default) {
              if (key == query) results.push(mod);
            }
          }

          for (key in mod) {
            if (key == query) results.push(mod);
          }
        } else if (typeof query === 'function') { 
          if (query(mod)) {
            results.push(mod);
          }
        } else {
          throw new TypeError('findModule can only find via string and function, ' + (typeof query) + ' was passed');
        }
        
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
const m = new moduleRaid();
const features = {};
const getABPropConfigValue = m.findModule("getABPropConfigValue")[0].getABPropConfigValue;
setTimeout(()=>{console.log("FeatureName               Original Value              new Value")}, 1000);
m.findModule("getABPropConfigValue")[0].getABPropConfigValue = function(e){
    const orgRet = getABPropConfigValue(e);
    let newValue = orgRet
    if(typeof orgRet==="boolean"){
        newValue = true;
    }
    if(features[e]===undefined){
        console.log("[+] "+e+"     "+orgRet+"      "+newValue);
    }
    features[e] = orgRet;
    return newValue;
}
```
