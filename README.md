# AG Filters Registry [![Build Status](https://travis-ci.org/AdguardTeam/FiltersRegistry.svg?branch=master)](https://travis-ci.org/AdguardTeam/FiltersRegistry)

This repository contains the known filters subscriptions available to AdGuard users. We re-host these filters on `filters.adtidy.org`. Also, these filters can be slightly modified in order to achieve better compatibility with AdGuard.

## Filters metadata

- `template.txt`

    Template file is used by the filters compiler to prepare the final filter version.
 
- `exclude.txt`

    A list of regular expressions. Rules that match these exclusions will not be included in the resulting filter.

- `metadata.json`

    Filter metadata. Includes name, description, etc.

    * `filterId` - unique filter identifier (integer);
    * `name` - filter name. Can be localized (we'll cover it later);
    * `description` - filter description;
    * `timeAdded` - time when this filter was added to the registry. Milliseconds since January 1, 1970. You can exec `new Date().getTime()` in the browser console to get the current time;
    * `homepage` - filter website/homepage;
    * `expires` - filter's default expiration period;
    * `displayNumber` - this number is used when AdGuard sorts available filters (GUI);
    * `groupId` - group identifier (see groups description below);
    * `subscriptionUrl` - default filter subscription URL
    * `tags` - a list of tags (see tags description below)

    Metadata example:
    ```javascript
    {
      "filterId": 2,
      "name": "English Filter",
      "description": "EasyList + AdGuard English filter. This filter is necessary for quality ad blocking.",
      "timeAdded": 1404115015843,
      "homepage": "https://kb.adguard.com/en/general/adguard-ad-filters#english",
      "expires": "2 days",
      "displayNumber": 1,
      "groupId": 1,
      "subscriptionUrl": "https://filters.adtidy.org/extension/chromium/filters/2.txt",
      "tags": [
        "purpose:ads",
      "reference:101",
      "recommended",
      "reference:2"
      ]
    }
    ```

- `revision.json`

  Filter version metadata, automatically filled and overwritten on each build.

- `filter.txt`

  Resulting compiled filter.

- `diff.txt`

  Build log that contains excluded and converted rules with an explanation.

### domains-blacklist.txt

A list of domains to be removed from url-blocking domain modified rules.

### Tags

Every filter can be marked by a number of tags. `/tags/metadata.json` contains every tag metadata.

Example:
```
  {
    "tagId": 1,
    "keyword": "purpose:ads"
  },
```

* `lang:*` tags

  Language-specific filters are marked with one or multiple `lang:` tags. For instance, AdGuard Russian filter is marked with the `lang:ru` tag.

* `purpose:*` tags

  Determines filters purposes. Please note, that a filter can have multiple purposes. For instance, `List-KR` is marked with both `purpose:ads` and `purpose:privacy`.

* `recommended` tag

  Filters that are recommended to use in their category. The category is determined by the pair of the `lang:` and `purpose:` tags.

### Groups

`/groups/metadata.json` - filters groups metadata. Each filter should belong to one of the groups.

## Filters localization

Translations are here: https://adguard.oneskyapp.com/collaboration/project?id=119172

See scripts/oneskyapp/README.md.

## How to build

```
npm install
```

Run the following command:
```
  node index.js
```
Build with white/black lists:
```
  node index.js -i=1,2,3 -s=4,5,6
```

Validate `filters.json` and `filters_i18n.json` for platforms:
```
  node validate ./platforms
```