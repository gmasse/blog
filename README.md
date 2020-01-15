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
