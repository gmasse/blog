[![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa]

## How to edit and preview this blog

### Requirements
- NodeJS

### Installation
Cloning source code:
```
git clone --recurse-submodules https://github.com/gmasse/blog.git
```
Installing dependencies:
```
cd blog
npm install
```
Set relative path of `node_module/` folder:
```
export PATH="$PATH:./node_modules/.bin"
```
Generating site and launching webserver:
```
hexo generate
hexo server
```


--------------------------------------------------------
This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa].

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg
