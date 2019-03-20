Documentation Of MaixPy
===========


|[Read me online](https://maixpy.sipeed.com/en/)  | [在线阅读](https://maixpy.sipeed.com/zh/) |
| ------------------------ | ----------- |

[![Build Status](https://travis-ci.org/sipeed/MaixPy_DOC.svg?branch=master)](https://travis-ci.org/sipeed/MaixPy_DOC)



-----------------------------------------------------------------------

## Build Doc


This documentation site is powered by GitBook. You can check out the online version here.

You need Node.js and npm to be able to build the site.

To install gitbook:

```
npm install gitbook-cli -g
```

Get Doc source code:
```
sudo apt install git 
git clone https://github.com/sipeed/MaixPy_DOC.git
```

Install gitbook plugins:

```
cd MaixPy_DOC
gitbook install
```

Serve as a website:

```
chmod +x serve.sh
./serve.sh
```

Then visit http://localhost:4000

