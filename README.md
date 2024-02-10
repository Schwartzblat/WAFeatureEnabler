# WAFeatureEnabler
Script that enables all AB features in WhatsApp Web.

> [!WARNING] 
> This JavaScript code is for **research purposes** only and is not intended to be used as a basis for any commercial or non-research purposes.  
> The use of this code is at the user's own risk, and the author(s) assume no responsibility for any misuse or unintended consequences.  
>
> 
> **By using this code, the user acknowledges and agrees to the following:**
> 
> The user is solely responsible for any legal consequences arising from the use of this code.
>   
> The user will not hold the author(s) liable for any damages, losses, or legal actions resulting from the use of this code.
> 
> The user agrees to comply with any applicable laws and regulations, including but not limited to copyright, intellectual property, and privacy laws.
> 
> Furthermore, the user acknowledges that using this code may violate WhatsApp's Terms of Service and could potentially lead to account bans or other penalties.
> 
> The user is advised to consult their own legal counsel before using this code to ensure compliance with all applicable laws and regulations.  

## Usage
1. Open WhatsApp Web in your browser.
2. Press F12 to open the developer tools.
3. Go to the console tab.
4. Paste the script.
```js
const moduleRaid = (() => {
    const mObj = {};
    window.webpackChunkwhatsapp_web_client.push([
        ["moduleRaid"],
        {},
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
            if (typeof query !== "function" && typeof query !== "string") {
                return;
            }
            if (typeof query === "function" && query(mod)) {
                results.push(mod);
                return;
            }
            for (const key in (mod === null || mod === void 0 ?
                    void 0 :
                    mod.default) || mod) {
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
moduleRaid.findModule("getABPropConfigValue")[0].getABPropConfigValue =
    function(featureName) {
        const retVal = getABPropConfigValue(featureName);
        let newValue = retVal;
        const featureList = [
            "edit",
            "bonsai",
            "community",
            "web",
            "username",
            "report",
            "receive",
            "wab",
            "view",
            "enabled"
            /*, "feature name" ...*/
        ];
        if (featureList.some((element) => featureName.includes(element))) {
            newValue = true;
        }
        if (features[featureName] === undefined) {
            console.log(
                `[+] ${featureName.padEnd(30)} ${retVal
          .toString()
          .padEnd(30)} ${newValue.toString()}`
            );
        }
        features[featureName] = retVal;
        return newValue;
    };
```
