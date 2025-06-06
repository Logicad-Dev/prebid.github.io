---
layout: page_v2
page_type: module
title: GPP Control - US States
description: If your CMP populates sections 8-12 of the Global Privacy Platform, this module complements the `consentManagementGpp` module to control Prebid.js accordingly.
module_code : gppControl_usstates
display_name : GPP Control - US States
enable_download : true
recommended: true
min_js_version: 8.10.0
sidebarType : 1
---

# GPP Control Module - US state strings
{: .no_toc }

- TOC
{: toc }

{% include legal-warning.html %}

## Overview

This consent management control module is designed to support the [Global Privacy Platform](https://iabtechlab.com/gpp/) US state strings, [GPP sections](https://github.com/InteractiveAdvertisingBureau/Global-Privacy-Platform/blob/main/Sections/Section%20Information.md) 8 through 12.
It works by translating them into an equivalent US national string as detailed in [Interpreting USNat strings](/features/mspa-usnat.html#interpreting-usnat-strings), and using it to apply the same [activity restrictions](/features/mspa-usnat.html#usnat-activity-restrictions). Prebid.js encourages publishers to consider with their legal counsel use of the national string over state strings. New state strings are being released, yet national string support is mature and less error prone.

{: .alert.alert-warning :}
Prebid functionality created to address regulatory requirements does not replace each party's responsibility to determine its own legal obligations and comply with all applicable laws. **We recommend consulting with your legal counsel before determining how to utilize these features in support of your overall privacy approach. This module is not intended to replace other consent modules; it supplements them.**

## Page Integration

By default, including this module enables activity restrictions for GPP sections from ID 8 to 12, as long as the [consentManagementGpp](/dev-docs/modules/consentManagementGpp.html) module is also included and configured.
Optional configuration options are:

{: .table .table-bordered .table-striped }
| Param | Type | Description | Example |
| --- | --- | --- | --- |
| gpp.mspa | `Object` | | |
| gpp.mspa.sids | `Array` | GPP SIDs that should be covered by activity restrictions. Defaults to all US state SIDs (`[8, 9, 10, 11, 12]`). This is the only value needed for normal operation. Other options are for special cases and future-proofing. | `[8, 9]` |
| gpp.mspa.sections | `Object` | Map from section ID to per-section configuration options | `{8: {name: 'usca'}}` |
| gpp.mspa.sections.name | `String` | GPP API name to use for the section. Defaults to the names given listed under [section information -> section IDs](https://github.com/InteractiveAdvertisingBureau/Global-Privacy-Platform/blob/main/Sections/Section%20Information.md#section-ids). This option would only be used if your CMP has named their sections in a non-standard way. | `uscav3` |
| gpp.mspa.sections.normalizeAs | `integer` | Normalize the flags for this section as if it were the number provided. See [example](#normalize-example). Each section defaults to its own ID. | `8` |

### Examples

#### Enable activity controls for certain sections only

```javascript
pbjs.setConfig({
  consentManagement: {
      gpp: {
          mspa: {
              sids: [8, 9] // enable only for CA and VA
          }
      }
  }
})
```

#### Non-standard GPP API names

If the CMP has a non-standard label for a given section, the `name` parameter may be used:

```javascript
pbjs.setConfig({
  consentManagement: {
    gpp: {
      mspa: {
        sections: {
          8: {
            name: 'uspcav1' // Use 'uspcav1' instead of 'usca' to retrieve section 8 from the CMP
          }
        }
      }
    }
  }
})
```

#### Select normalization rule

<a id="normalize-example"></a>

This module provides a [normalization algorithm](/features/mspa-usnat.html#interpreting-usnat-strings) for each section from 8 to 12, which it uses to translate each section's data into its equivalent section 7 (usnat) representation.
It is possible to re-use them for other sections; this can be useful if new GPP sections are released and they happen to follow the same format as an exising one. For example, if a new US GPP SID 51 comes along that should be read from the CMP as 'uss51' and normalized the same as California's string, here's the config:

```javascript
pbjs.setConfig({
  consentManagement: {
    gpp: {
      mspa: {
        sids: [51],       // SID for the 51st state
        sections: {
          51: {
            // expect the CMP to provide section 51, with API name 'uss51', and the same format as section 8 (CA)
            name: 'uss51',
            normalizeAs: 8 
          }
        }
      }
    }
  }
})
```

## Build the Package

Follow the basic build instructions in the GitHub Prebid.js repo's main [README](https://github.com/prebid/Prebid.js/blob/master/README.md). To include the consent management module and the GPP Control - US states module, an additional option must be added to the **gulp build** command:

```bash
gulp build --modules=consentManagementGpp,gppContol_usstates,bidAdapter1,bidAdapter2
```

You can also use the [Prebid.js Download](/download.html) page.

## Further Reading

- [IAB Global Privacy Platform Full Specification Repository](https://github.com/InteractiveAdvertisingBureau/Global-Privacy-Platform)
- [IAB Global Privacy Platform CMP API Specification](https://github.com/InteractiveAdvertisingBureau/Global-Privacy-Platform/blob/main/Core/CMP%20API%20Specification.md)
- [IAB Global Privacy Platform USNat string Specification](https://github.com/InteractiveAdvertisingBureau/Global-Privacy-Platform/blob/main/Sections/US-National/IAB%20Privacy%E2%80%99s%20Multi-State%20Privacy%20Agreement%20(MSPA)%20US%20National%20Technical%20Specification.md
- [Prebid US Compliance Support](/features/mspa-usnat.html)
- [Prebid Activity Controls](/dev-docs/activity-controls.html)
- [Prebid Consent Management - US Privacy Module](/dev-docs/modules/consentManagementUsp.html)
- [Prebid Consent Management - GPP Module](/dev-docs/modules/consentManagementGpp.html)
- [Prebid Consent Management - GPP Control - USNat module](/dev-docs/modules/gppControl_usnat.html)
- [Prebid Activity Controls -- GPP control module - USNat](/dev-docs/modules/gppControl_usnat.html)
- [CMP Best Practices](https://docs.prebid.org/dev-docs/cmp-best-practices.html)
