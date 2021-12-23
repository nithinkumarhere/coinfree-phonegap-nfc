Coinfree PhoneGap NFC Plugin
==========================

The NFC plugin allows you to read and write  NFC tags. You can also beam to, and receive from, other NFC enabled devices.

Use to
* read data from NFC tags
* write data to NFC tags
* send data to other NFC enabled devices
* receive data from NFC devices
* send raw commands (ISO 14443-3A, ISO 14443-3A, ISO 14443-4, JIS 6319-4, ISO 15693) to NFC tags

This plugin uses NDEF (NFC Data Exchange Format) for maximum compatibilty between NFC devices, tag types, and operating systems.

Supported Platforms
-------------------
* Android
* [iOS 11](#ios-notes)
* Windows (includes Windows Phone 8.1, Windows 8.1, Windows 10)
* BlackBerry 10
* Windows Phone 8
* BlackBerry 7

## Contents

* [Installing](#installing)
* [NFC](#nfc)
* [NDEF](#ndef)
  - [NdefMessage](#ndefmessage)
  - [NdefRecord](#ndefrecord)
* [Events](#events)
* [Platform Differences](#platform-differences)
* [BlackBerry 10 Invoke Target](#blackberry-10-invoke-target)
* [Launching Application when Scanning a Tag](#launching-your-android-application-when-scanning-a-tag)
* [Testing](#testing)
* [Sample Projects](#sample-projects)
* [Host Card Emulation (HCE)](#hce)
* [Book](#book)
* [License](#license)

# Installing

### ionic capacitor

    $ npm install git@github.com:nithinkumarhere/coinfree-phonegap-nfc.git

# NFC

> The nfc object provides access to the device's NFC sensor.

## Methods

- [nfc.addNdefListener](#nfcaddndeflistener)
- [nfc.write](#nfcwrite)
- [nfc.makeReadOnly](#nfcmakereadonly)

## nfc.addNdefListener

Registers an event listener for any NDEF tag.

    nfc.addNdefListener(callback, [onSuccess], [onFailure]);

### Parameters

- __callback__: The callback that is called when an NDEF tag is read.
- __onSuccess__: (Optional) The callback that is called when the listener is added.
- __onFailure__: (Optional) The callback that is called if there was an error.

### Description

Function `nfc.addNdefListener` registers the callback for ndef events.

A ndef event is fired when a NDEF tag is read.

For BlackBerry 10, you must configure the type of tags your application will read with an [invoke-target in config.xml](#blackberry-10-invoke-target).

On Android registered [mimeTypeListeners](#nfcaddmimetypelistener) takes precedence over this more generic NDEF listener.

On iOS you must call [beingSession](#nfcbeginsession) before scanning a tag.

### Supported Platforms

- Android
- iOS
- Windows
- BlackBerry 7
- BlackBerry 10
- Windows Phone 8

## nfc.write

Writes an NDEF Message to a NFC tag.

A NDEF Message is an array of one or more NDEF Records

    var message = [
        ndef.textRecord("hello, world"),
        ndef.uriRecord("http://github.com/chariotsolutions/phonegap-nfc")
    ];

    nfc.write(message, [onSuccess], [onFailure]);

### Parameters

- __ndefMessage__: An array of NDEF Records.
- __onSuccess__: (Optional) The callback that is called when the tag is written.
- __onFailure__: (Optional) The callback that is called if there was an error.

### Description

Function `nfc.write` writes an NdefMessage to a NFC tag.

On **Android** this method *must* be called from within an NDEF Event Handler.

On **iOS** this method can be called outside the NDEF Event Handler, it will start a new scanning session. Optionally you can reuse the read session to write data. See example below.

On **Windows** this method *may* be called from within the NDEF Event Handler.

On **Windows Phone 8.1** this method should be called outside the NDEF Event Handler, otherwise Windows tries to read the tag contents as you are writing to the tag.

### Examples

#### Android

On Android, write must be called inside an event handler

    function onNfc(nfcEvent) {
    
        console.log(nfcEvent.tag);
        
        var message = [
            ndef.textRecord(new String(new Date()))
        ];
        
        nfc.write(
            message,
            success => console.log('wrote data to tag'),
            error => console.log(error)
        );

    nfc.addNdefListener(onNfc);


#### iOS - Simple

Calling `nfc.write` on iOS will create a new session and write data when the user taps a NFC tag

        var message = [
            ndef.textRecord("Hello, world")
        ];

        nfc.write(
            message,
            success => console.log('wrote data to tag'),
            error => console.log(error)
        );

#### iOS - Read and Write

On iOS you can optionally write to NFC tag using the read session

        try {
            let tag = await nfc.scanNdef({ keepSessionOpen: true});

            // you can read tag data here
            console.log(tag);
            
            // this example writes a new message with a timestamp
            var message = [
                ndef.textRecord(new String(new Date()))
            ];

            nfc.write(
                message,
                success => console.log('wrote data to tag'),
                error => console.log(error)
            );

        } catch (err) {
            console.log(err);
        }

### Supported Platforms

- Android
- iOS
- Windows
- BlackBerry 7
- Windows Phone 8

## nfc.makeReadOnly

This is a exclusive method for CoinFree APP, Makes a third read on NFC Tag.  **Warning this is only for CoinFree.**

    nfc.makeReadOnly([onSuccess], [onFailure]);

### Parameters

- __onSuccess__: (Optional) The callback that is called when the tag is locked.
- __onFailure__: (Optional) The callback that is called if there was an error.

### Description

Function `nfc.makeReadOnly` make a NFC tag read with android native api such as getNdefMessage.

### Supported Platforms

- Android


License
================

The MIT License

Copyright (c) 2011-2020 Chariot Solutions

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
