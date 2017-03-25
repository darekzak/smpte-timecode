# smpte-timecode

`smpte-timecode is a JavaScript library for operations with [SMPTE timecodes](https://en.wikipedia.org/wiki/SMPTE_timecode).

## Features

- usable browser or Node environment;
- supports drop-frame and non-drop-frame codes;
- instantiate timecodes from frame count, string time code or JavaScript Date() objects;
- timecode arithmetics: adding frame counts and other timecodes;
- support of implicit conversiont to `string` (`toString()`) and `number` (`valueOf()`);

## Usage

```javascript
var t = Timecode('00:15:10;03');
t.add('00:02:30;00;');
console.log(t.frameCount);
t.subtract(100); //frames
console.log(t.toString());
```

## Creating Timecode() Objects
```javascript
Timecode = function (timecode, frameRate, dropFrame) {...};

// Examples:
var minute = new Timecode('00:01:00:00');
var eightHundredFrames = new Timecode(800,29.97,true);
var wallClock = new Timecode(new Date());
```
<dl>
    <dt>timecode</dt>
    <dd>Can be a number, string, or a Date() object. Numbers are interpreted as
        frame count. Strings are expected as `"HH:MM:SS:FF"` (non-drop-frame) or
        `"HH:MM:SS;FF"` (drop-frame). If Date() is passed, it is converted to the
        timecode a master clock would have with a given framerate, month date and 
        year discarded.
    </dd>
    <dt>frameRate</dt>
    <dd>(optional) A number, one of 24, 25, 29.97, 30, 50, 60 is expected. 
        29,97 is assumed if the parameter is omitted.
    </dd>
    <dt>dropFrame<dt>
    <dd>(optional) Boolean, if omitted, drop-frame mode is determined based on
        the ":" or ";" characters separating the frames in the timecode parameter.
        If timecode parameter is non-string, drop-frame assumed for 29.97 fps,
        non-drop-frame for all other framerates.
    </dd>
</dl>

Note: a direct call to `Timecode()` returns a `Timecode` object too, so both direct
calls and instantiating with `new` return the same result:
```javascript
console.log((new Timecode('00:15:00;00')).toString()); // is the same as
console.log(Timecode('00:15:00;00').toString());
```

## Using Timecode() Objects

Once a `Timecode` object is created, the following member variables are available:

- `frameCount`: number, total number of frames
- `frameRate`: number, framerate in FPS
- `hours`: number
- `minutes`: number
- `seconds`: number
- `frames`: number
- `dropFrame`: boolean, whether timecode is drop-frame or not

The `Timecode` object also provides the following member functions:

- `add(x)`: Timecode, adds `x` to timecode, `x` can be a number, `Date` or `Timecode`
- `subtract(x)`: Timecode, subtracts `x` from timecode, `x` can be a number, `Date` 
    or `Timecode`
- `toString()`: string, returns the timecode in "HH:MM:SS:FF" or "HH:MM:SS;FF" format
- `toDate()`: date, returns a `Date` object using today's date and timecode as wall clock
- `valueOf()`: number, returns `this.frameCount`

For more usage examples, see the unit tests.

## Running Tests
To run tests, make sure you install `expect.js`, `mocha` and `istanbul` NPMs **locally**.

    npm install expect.js mocha istanbul

The tests can be run in Node using:

    npm test

To run the tests in a browser environment, open the `test/smpte-timecode-test.html` file
in a browser.

## Credits
- [https://www.npmjs.com/package/timecode](https://www.npmjs.com/package/timecode) 
NPM module, which in the end I decided to rewrite. The things I needed would have 
been breaking changes for anyone who used it.
- [http://andrewduncan.net/timecodes/](http://andrewduncan.net/timecodes/) by Andrew Duncan

## Legal
License: MIT  
Copyright &copy; 2017 Crystal Computer Corp.   
[http://www.crystalcc.com](http://www.crystalcc.com)
