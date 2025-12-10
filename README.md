# Papaya
This repository contains various assets which are used by the [Papaya Games Launcher](https://play.google.com/store/apps/details?id=com.sebschaef.papaya) Android app:
- Translations
- Icons for the various supported gaming platforms
- Configuration for the various gaming platforms, supported emulators and game streaming apps

You are more than welcome to contribute to any of these assets :)

To contribute, you can either directly open a Pull Request with your additions or you can send me them somewhere else and I'll add them to this Repository.

## Translations
How to add translations for another Locale:
1. Copy the translations file of any other existing Locale (`default.xml` = `en`)
2. Name the file according to your Locale's country code ([See here](https://saimana.com/list-of-country-locale-code/), but with a `-` instead of `_`). E.g. `pt` = Portugese (any country), `pt-BR` = Brazilian Portugese.
3. Translate the texts:
	- Translate only `<string "...">[this part]</string>` and do not modify the `name` etc
	- Translations marked with `translatable="false"` do not need to be translated. You can remove the whole line then.
	- `%1$s`, `%d` etc are placeholders for texts which are filled at runtime (`%1$s games` -> `PSP games`). Please keep them and place them where it makes sense in your language.

If you don't know how to use Git, you can also download a translation file, translate it and send it to me somewhere else. Also, don't hesistate to reach out to me if you need more information to contribute with translations. Thank you for contributing! :)
- [mail@sebschaef.com](mailto:mail@sebschaef.com)
- [Discord](https://discord.gg/bv9HTAepTP)
- [Reddit](https://www.reddit.com/user/IllegalArgException/)

## Icons
Any new icons should match the current style:
- Monochrome
- Relatively simple, not too detailled
- They should be recognizable even if they are displayed in a small size

The art style: 
- The current icons usually show the controller for the platform or the device itself if it is a Handheld
- Usually, the icon resembles the shape with cutouts where the screen is located and where the analog sticks are located. Consider this just as a guideline, because for older consoles without screens and analog sticks this scheme does not work.

## Config - Presets
The `presets.json` file is split into three parts:
- `platforms`: Gaming platforms. E.g. PlayStation Portable, Android etc.
- `emulatorApps`: The supported Emulators. E.g. PPSSPP, Dolphin etc.
- `streamingApps`: The supported Apps for the Game Streaming section.

See the sections below for more details about each part.

### Platforms
`platforms` is a list of objects, where each object refers to a gaming platform.

#### Platform
Each platform defintion contains some Metadata like the name of the gaming platform. Also, it contains detailled information of the supported game file types ("Roms") and references to their respective IDs on various Scraper APIs.

- **`key` (Required):** The internal identifier for this platform. Needs to be unique and should be at max 4 characters long (Preferred: 3).
- **`shortName` (Required):** Abbreviation of the platform which is visible to the user. Should be uppercase and at max 4 characters long (Preferred: 3).
- **`name` (Required):** Full name of the platform which is visible to the user. Should be according to this scheme: "\[Manufacturer\] \[Name\] (\[Alternative name\])"
- **`supportedFiles` (Required):** List of supported file types. See below.
- **`scraperPlatformId`:** Obkject with references to IDs of this platforms on the various Scraper APIs. See below.

Example:
```json
{
    "key": "psp",
    "shortName": "PSP",
    "name": "Sony PlayStation Portable",
    "supportedFiles": [...],
    "scraperPlatformId": ...
}
```

#### SupportedFile
Defines rules for a file type.

**- `type` (Required):** The file type (Aka the letters behind the dot). Should be lowercase for consistency, but the case is ignored by Papaya.
**- `nonDescriptiveNames`:** A list of file names (without the type) which don't hold any useful information for the scrapers. If such a name is matched, Papaya considers this file as a game, but will try to get of the search term for scraping from somwhere else (See samples below)
**- `ignoredNames`:** A list of file names (without the type). If such a name is matched, Papaya does not consider this file as a game file -> The file is ignored.
**- `idBy`:** If set, the value must be either `content` or `folder`. If this is set, Papaya considers a matching file not as a Rom, but as a definition of a Game ID. Depending on the set value, Papaya tries to extract Game ID either from the file `content` or the `folder` name in which the file lies (See samples below).

**Examples:**

A simple Rom file
```json
{
    "type": "iso"
}
```

A Rom file with occasionally not useful names. For example, the standard executable file for PSP games had the name `EBOOT.PBP` which is why they often might be found like this: `Driver 76/EBOOT.PBP`. With the definition below, we tell Papaya that the file name is not useful and we will try to get the game name from the folder name instead.
```json
{
    "type": "pbp"
    "nonDescriptiveNames": [
        "eboot",
        "param"
    ]
}
```

For very common file types, we might want to ignore a file if it has a certain name. For example, for PC games on GameHub Lite we can define `txt` as a valid Game ID type where we can find the ID inside the file and extract the game name from the file name (e.g. `GTA IV.txt`). However, the user might use the same folder with other Frontends like ES-DE, so we would like to ignore the `systeminfo.txt` which could be located in the same folder.
```json
{
    "type": "txt",
    "ignoredNames": [
        "systeminfo"
    ],
    "idBy": "content"
}
```

#### ScraperPlatformId
An optional object which contains the IDs of the platform on various scraper platforms. All fields are optional. If an ID for a scraper is set, Papaya saves an extra network call to get the platform by name: Less load for the API, shorter scraping time for the user.

**- `igdb`:** The ID of the platform on the "Internet Games Database".

**- `tgdb`:** The ID of the platform on the "The Games DB".

**- `rawg`:** The ID of the platform on Rawg.

### EmulatorApps
TBD


### StreamingApps
TBD
