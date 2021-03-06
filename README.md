timing.js
=========

> Timing.js is a small set of helpers for working with the [Navigation Timing API](https://developer.mozilla.org/en-US/docs/Navigation_timing) to identify where your application is spending its time. Useful as a standalone script, DevTools Snippet or bookmarklet.

## Features

* Normalizes `firstPaint` across Chrome, Opera and IE11 to `timing.getTimes().firstPaint`. Firefox may be able to do similar with `MozAfterPaint`
* Adds `firstPaintTime` (`firstPaint` - load/nav start)
* Adds:`domReadyTime`, `initDomTreeTime`, `loadEventTime`, `loadTime`, `redirectTime`, `requestTime`, `uploadEventTime` `connectTime`

## Installation

### Clone

Download the [latest](https://github.com/addyosmani/timing.js/archive/master.zip) version or just `git clone https://github.com/addyosmani/timing.js.git`.

### Bookmarklet:

```javascript
javascript:(function(c){c.timing=c.timing||{getTimes:function(e){var a=(c.performance||c.webkitPerformance||c.msPerformance||c.mozPerformance).timing,b={};e=e||{};if(a){if(e&&!e.simple)for(var d in a)a.hasOwnProperty(d)&&(b[d]=a[d]);void 0===b.firstPaint&&(d=0,c.chrome&&c.chrome.loadTimes?(d=1E3*c.chrome.loadTimes().firstPaintTime,b.firstPaintTime=d-1E3*c.chrome.loadTimes().startLoadTime):"number"===typeof c.performance.timing.msFirstPaint&&(d=c.performance.timing.msFirstPaint,b.firstPaintTime=d-c.performance.timing.navigationStart),e&&!e.simple&&(b.firstPaint=d));b.loadTime=a.loadEventEnd-a.navigationStart;b.domReadyTime=a.domComplete-a.domInteractive;b.readyStart=a.fetchStart-a.navigationStart;b.redirectTime=a.redirectEnd-a.redirectStart;b.appcacheTime=a.domainLookupStart-a.fetchStart;b.unloadEventTime=a.unloadEventEnd-a.unloadEventStart;b.lookupDomainTime=a.domainLookupEnd-a.domainLookupStart;b.connectTime=a.connectEnd-a.connectStart;b.requestTime=a.responseEnd-a.requestStart;b.initDomTreeTime=a.domInteractive-a.responseEnd;b.loadEventTime=a.loadEventEnd-a.loadEventStart}return b},printTable:function(c){var a={},b=this.getTimes(c);Object.keys(b).sort().forEach(function(c){a[c]={ms:b[c],s:+(b[c]/1E3).toFixed(2)}});console.table(a)},printSimpleTable:function(){this.printTable({simple:!0})}};return timing.printSimpleTable()})(this);
```

### Bower:

```sh
$ bower install timing-js
```

### npm:

```sh
$ npm install timing.js
```

## Usage

By default, running the script will print out a summary table of measurements. The API for the script is as follows:

Get measurements:

```sh
timing.getTimes();
```

Print a summary table of measurements (uses [console.table()](https://plus.google.com/+AddyOsmani/posts/PmTC5wwJVEc)):

```sh
timing.printSimpleTable();
```

![](http://i.imgur.com/zjEST62.png)

Also works in Firefox DevTools (Firefox Nightly only for now):

![](http://i.imgur.com/jY3xHi3.png)

Print a complete table of measurements (including rest of `window.performance`):

```sh
timing.printTable();
```

![](http://i.imgur.com/C9eRQe9.png)


### Sample output of `timing.getTimes()`

Chrome:

```javascript
firstPaint: 1411307463455.813 // New
firstPaintTime: 685.0390625 // New
appcacheTime: 2
connectEnd: 1411307463185
connectStart: 1411307463080
connectTime: 105 // New
domComplete: 1411307463437
domContentLoadedEventEnd: 1411307463391
domContentLoadedEventStart: 1411307463391
domInteractive: 1411307463391
domLoading: 1411307463365
domReadyTime: 46 // New
domainLookupEnd: 1411307463080
domainLookupStart: 1411307463032
fetchStart: 1411307463030
initDomTreeTime: 56 // New
loadEventEnd: 1411307463445
loadEventStart: 1411307463437
loadEventTime: 8 // New
loadTime: 558 // New
lookupDomainTime: 48
navigationStart: 1411307462887
readyStart: 143 // New
redirectEnd: 0
redirectStart: 0
redirectTime: 0 // New
requestStart: 1411307463185
requestTime: 150 // New
responseEnd: 1411307463335
responseStart: 1411307463333
secureConnectionStart: 1411307463130
unloadEventEnd: 0
unloadEventStart: 0
unloadEventTime: 0 // New
```

Firefox:

![](http://i.imgur.com/Drr4A6B.png)

IE 11:

![](http://i.imgur.com/ekVHk3P.png)
