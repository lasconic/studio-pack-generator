# Studio-Pack-Generator

This project convert a folder or a RSS URL to
[Studio](https://github.com/marian-m12l/studio) pack zip for Lunii device, see
file structure below.

Supported OS: Windows / Linux / macOS

**[⭐ Une grosse communauté est présente sur Discord pour créer et partager des pack Lunii ! ⭐](https://discord.com/invite/jg9MjHBWQC)**

**[⭐ A big french community is present on Discord to create and share Lunii packs ⭐](https://discord.com/invite/jg9MjHBWQC)**

## Quick start

```shell
studio-pack-generator "my story folder OR a RSS URL"
```

will generate "my story folder-xxxxxxxxxx.zip" that can be imported in
[Studio](https://github.com/marian-m12l/studio)

Examples:

- `studio-pack-generator http://radiofrance-podcast.net/podcast09/rss_19721.xml`
- `studio-pack-generator "Musics"`
- `studio-pack-generator "Encore une histoire"`

## Optional dependencies

- **ffmpeg** : used to extract images from story mp3 files, increase volume of
  files, convert to the right format.
  <br>→ Use `--skip-audio-convert` and `--skip-extract-image-from-mp3` to avoid
  this usage.
- **imagemagick** : used to generate menu image files.
  <br>→ Use `--skip-image-item-gen` to avoid this usage.
- **picoTTS** : used to generate menu audio files.
  <br>→ Use `--skip-audio-item-gen` to avoid this usage.

Install optional dependencies :
`sudo apt update && sudo apt install -y ffmpeg libttspico-utils imagemagick`

[Windows release](https://github.com/jersou/studio-pack-generator/releases) of
studio-pack-generator embeds these tools in zip file, and use Windows TTS
instead of picoTTS (unless you have WSL and picoTTS installed).

Use "-miva" option to skip all generations that use these tools.

## Install studio-pack-generator

### Install binary from [release page](https://github.com/jersou/studio-pack-generator/releases) and run it :

```
studio-pack-generator-x86_64-linux            "my story folder or a rss url"
or  studio-pack-generator-x86_64-windows.exe  "my story folder or a rss url"
or  studio-pack-generator-aarch64-apple       "my story folder or a rss url"
or  studio-pack-generator-x86_64-apple        "my story folder or a rss url"
```

### Or use [Deno](https://deno.land/)

This project is written in Typescript for [deno](https://deno.land/) runtime.
Install deno : https://deno.land/#installation

#### Run from web directly (will be cached for the next launches) :

```
deno run -A https://raw.githubusercontent.com/jersou/studio-pack-generator/main/studio_pack_generator.ts "my story folder or a rss url"
```

#### Or install with deno :

```
deno install --name studio-pack-generator -A --unstable -f https://raw.githubusercontent.com/jersou/studio-pack-generator/main/studio_pack_generator.ts
→ and then run :  studio-pack-generator "my story folder or a rss url"
```

#### Or clone the repo and run with deno :

```
git clone https://github.com/jersou/studio-pack-generator
cd studio-pack-generator
deno run -A studio_pack_generator.ts "my story folder or a rss url"
```

## Story folder structure

Simplest example, only 1 menu level, without audio/image of menus/items :

```shell
📂 Story folder
└── 📂 Choose a story         ← 📂 first menu
    ├── 🎵 the story 1.mp3      ← 📗 audio story
    ├── 🎵 the story 2.mp3      ← 📗 audio story
    └── 🎵 the story 3.mp3      ← 📗 audio story
```

Simple example, 2 levels of menus, without audio/image of menus/items :

```shell
📂 Story folder
└── 📂 Choose a character          ← 📂 first menu
    ├── 📂 Alice                     ← 📂 first choice of the first menu
    │   └── 📂 Choose a place          ← 📂 second menu
    │       ├── 🎵 the city.mp3          ← 📗 audio story
    │       └── 🎵 the jungle.mp3        ← 📗 audio story
    └── 📂 Bob                       ← 📂 second choice of the first menu
        └── 📂 Choose a place          ← 📂 second menu
            ├── 🎵 the desert.mp3        ← 📗 audio story
            └── 🎵 the jungle.mp3        ← 📗 audio story
```

studio-pack-generator will generate menu files, they could be manually
overwritten, and the next studio-pack-generator run will not regenerate these
files :

```shell
📂 Story folder
├── 🎵 0-item.mp3                     ← ⏩ story audio title, generated if missing
├── 🔳 0-item.png                     ← ⏩ story image title, generated if missing
├── 🔳 0-night-mode.mp3               ← ⏩ story audio night mode transition, generated if missing and if the mode is enable
└── 📂 Choose a character             ← 📂 first menu
    ├── 🎵 0-item.mp3                   ← ⏩ audio menu, generated if missing
    ├── 📂 Alice                        ← 📂 first choice of the first menu
    │   ├── 🎵 0-item.mp3                 ← ⏩ audio choice, generated if missing
    │   ├── 🔳 0-item.png                 ← ⏩ image choice, generated if missing
    │   └── 📂 Choose a place             ← 📂 second menu
    │       ├── 🎵 0-item.mp3               ← ⏩ audio menu, generated if missing
    │       ├── 🔳 0-item.png               ← ⏩ audio menu, generated if missing
    │       ├── 🎵 the city.item.mp3        ← ⏩ audio story title, generated if missing
    │       ├── 🔳 the city.item.png        ← ⏩ image story title, generated if missing
    │       ├── 🎵 the city.mp3             ← 📗 audio story
    │       ├── 🎵 the jungle.item.mp3      ← ⏩ audio story title, generated if missing
    │       ├── 🔳 the jungle.item.png      ← ⏩ image story title, generated if missing
    │       └── 🎵 the jungle.mp3           ← 📗 audio story
    └── 📂 Bob                          ← 📂 second choice of the first menu
        ├── 🎵 0-item.mp3                 ← ⏩ audio choice, generated if missing
        ├── 🔳 0-item.png                 ← ⏩ image choice, generated if missing
        └── 📂 Choose a place                ← 📂 second menu
            ├── 🔳 0-item.mp3               ← ⏩ audio menu, generated if missing
            ├── 🔳 0-item.png               ← ⏩ audio menu, generated if missing
            ├── 🎵 the desert.item.mp3      ← ⏩ audio story title, generated if missing
            ├── 🔳 the desert.item.png      ← ⏩ image story title, generated if missing
            ├── 🎵 the desert.mp3           ← 📗 audio story
            ├── 🎵 the jungle.item.mp3      ← ⏩ audio story title, generated if missing
            ├── 🔳 the jungle.item.png      ← ⏩ image story title, generated if missing
            └── 🎵 the jungle.mp3           ← 📗 audio story
```

There is no limit to the nesting of menus, for example :

```shell
📂 Story folder
└── 📂 Choose a character                 ← 📂 first menu
    ├── 📂 Alice                            ← 📂 first choice of the first menu
    │   └── 📂 Choose a place                ← 📂 second menu
    │       └── 📂 Building                    ← 📂 second choice of the first menu
    │       │   └── 📂 Choose the floor          ← 📂 third menu
    │       │       ├── 🎵 the floor 1.mp3         ← 📗 audio story
    │       │       └── 🎵 the floor 2.mp3         ← 📗 audio story
    │       ├── 🎵 the city.mp3                ← 📗 audio story : mix menus/stories is possible
    │       └── 🎵 the jungle.mp3              ← 📗 audio story : mix menus/stories is possible
    ├── 🎵 Bob.mp3                         ← 📗 audio story : mix menus/stories is possible
    ...
```

### Zip Pack aggregation

_Since v0.1.11._

studio-pack-generator can embed zip studio packs in the tree structure :

```shell
📂 Story folder
└── 📂 Choose a character   ← 📂 first menu
    ├── 📦 Alice.zip           ← 📦 pack as menu entry
    ├── 🎵 Bob.mp3             ← 📗 audio story
    ...
```

The "super pack" will look like :

```shell
📂 Story folder
└── 📂 Choose a character                 ← 📂 first menu
    ├── 📂 Alice                            ← 📂 The Alice.zip pack
    │   └── 📂 Choose a place                ← 📂 second menu
    │       └── 📂 Building                    ← 📂 second choice of the first menu
    │       │   └── 📂 Choose the floor          ← 📂 third menu
    │       │       ├── 🎵 the floor 1.mp3         ← 📗 audio story
    │       │       └── 🎵 the floor 2.mp3         ← 📗 audio story
    │       ├── 🎵 the city.mp3                ← 📗 audio story
    │       └── 🎵 the jungle.mp3              ← 📗 audio story
    ├── 🎵 Bob.mp3                         ← 📗 audio story
    ...
```

## Tips

- The first digit of file/folder name are ignored, it's useful to sort
  stories/menus.
- To keep numbers in generated items : "- 3 petits cochons.mp3" or "12 - 3
  petits cochons.mp3".
- Image formats : png, jpg, bmp.
- Audio formats : mp3, ogg, opus, wav.

## Usage

```
deno run -A studio_pack_generator.ts [options] <story path | RSS URL>    convert a folder or a RSS URL to Studio pack

Options:
  -h, --help                         Show help                                                                 [boolean]
  -d, --add-delay                    add 1 second at the beginning and the end of audio files [boolean] [default: false]
  -n, --auto-next-story-transition   go to next story of group at end of stories              [boolean] [default: false]
  -b, --select-next-story-at-end     select the next story in the menu at end                 [boolean] [default: false]
  -l, --lang                         the lang used to generate menu and items. Auto detected by default         [string]
  -t, --night-mode                   enable night mode : add transitions to an uniq endpoint  [boolean] [default: false]
  -o, --output-folder                zip output folder                                                          [string]
  -c, --seek-story                   cut the beginning of stories: 'HH:mm:ss' format or 'N' sec                 [string]
  -v, --skip-audio-convert           skip convert audio (and skip increase volume)            [boolean] [default: false]
  -j, --skip-image-convert           skip convert image                                       [boolean] [default: false]
  -a, --skip-audio-item-gen          skip audio item generation                               [boolean] [default: false]
  -m, --skip-extract-image-from-mp3  skip extract item image from story mp3                   [boolean] [default: false]
  -i, --skip-image-item-gen          skip image item generation                               [boolean] [default: false]
  -s, --skip-not-rss                 skip all except download RSS files                       [boolean] [default: false]
  -r, --skip-rss-image-dl            skip RSS image download of items                         [boolean] [default: false]
  -w, --skip-wsl                     disable WSL usage                                        [boolean] [default: false]
  -z, --skip-zip-generation          only process item generation, don't create zip           [boolean] [default: false]
  -e, --use-open-ai-tts              generate missing audio item with Open AI TTS             [boolean] [default: false]
  -k, --open-ai-api-key              the OpenAI API key                                                         [string]
  -g, --open-ai-model                OpenAi model : tts-1, tts-1-hd                          [string] [default: "tts-1"]
  -p, --open-ai-voice                OpenAi voice : alloy, echo, fable, onyx, nova, shimmer   [string] [default: "onyx"]
  -x, --extract                      extract a zip pack (reverse mode)                        [boolean] [default: false]
```

Separate options by spaces, ex :

- short version : `studio_pack_generator -v -j -a "the story"` or
  `studio_pack_generator -vja "the story"`
- long version :
  `studio_pack_generator --skip-audio-convert --skip-image-convert --skip-audio-item-gen "the story"`

## Features

- Generate studio pack from file tree.
- Generate menu image/audio file if missing.
- Extract image from mp3 file as story image in menu.
- Increase audio volume of stories if needed.
- Download podcast from a RSS url and generate the story tree, cut by parts of
  10 stories.
- Convert mp3 files to right format (mp3, 44100 Hz, mono).
- Convert image files to right format (320x240).
- Generate story thumbnail.
- Option to chaining the stories.
- Option enable the night mode.
- Option to add 1 sec of silence at the beginning and end of sound files.
- Option to skip the beginning of stories.
- Zip Pack aggregation

### Overwrite metadata

If the file `metadata.json` exists in the story folder, it will be used to
overwrite the `story.json` metadata.

All key/value are optional, ex:

```json
{
  "title": "title - overwrite",
  "description": "description - overwrite",
  "format": "v1",
  "version": 1,
  "nightModeAvailable": false
}
```

## OpenAI TTS

To use OpenAI TTS, use `--use-open-ai-tts` option, and you must set the API key:

- set OPENAI_API_KEY in the environnement variables
- or use --open-ai-api-key parameter
- or enter the key when the program prompt

## reverse process : extract pack from zip

Extract a file stucture from zip pack :

```shell
-x, --extract                      extract a zip pack (reverse mode)                        [boolean] [default: false]
```

Example :

```shell
studio-pack-generator -x 2-full.zip
```

or

```shell
studio-pack-generator -x -o output/dir  2-full.zip
```

Note: it doesn't work well with "menu" nodes and with pack without "question"
stage.

## Development

Some dev command are listed in the deno.json file :

- bundle: bundle the project and its dependencies to
  `dist/studio_pack_generator.js`
- fmt: format the code
- gen-bin: generate the binaries
- gen-cov: generate the test coverage
- lint: lint the code
- pre-commit: fmt && lint && test
- pre-commit-fast: parallel pre-commit task
- start: run studio_pack_generator.ts
- test: launch tests
- test-watch: launch tests on file change
- vendor: vendor the dependencies to `vendor`

Usage : `deno task <command>`, ex : `deno task fmt`

## Possible improvements

- https://github.com/jersou/studio-pack-generator/issues/19 : end node
- clean file download option unless -z
- ...
