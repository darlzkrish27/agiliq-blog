Title: Comparison of mobile app frameworks: Iphone, Java, Phonegap and Titanium
Date: 2011-02-08 12:29:16
Modified: 2011-02-08 23:59:16+05:30
Slug: comparision-iphone-android-phonegap-titanium
Authors: shabda
Tags: java
Summary: <style>.tblGenFixed td {padding:0 3px;overflow:hidden;white-space:normal;letter-spacing:0;word-spacing:0;background-color:#fff;z-index:1;border-top:0px none;border-left:0px none;border-bottom:1px solid #CCC;border-right:1px solid #CCC;} .dn {display:none} .tblGenFixed td.s0 {background-color:white;font-family:arial,sans,sans-serif;font-size:100.0%;font-weight:normal;font-style:normal;color:#000000;text-decoration:none;text-align:left;vertical-align:bottom;white-space:normal;overflow:hidden;text-indent:0px;padding-left:3px;border-top:1px solid #CCC;border-right:;border-bottom:;border-left:1px solid #CCC;} .tblGenFixed td.s2 {background-color:white;font-family:arial,sans,sans-serif;font-size:100.0%;font-weight:normal;font-style:normal;color:#000000;text-decoration:none;text-align:left;vertical-align:bottom;white-space:normal;overflow:hidden;text-indent:0px;padding-left:3px;border-right:;border-bottom:;border-left:1px solid #CCC;} .tblGenFixed td.s1 {background-color:white;font-family:arial,sans,sans-serif;font-size:100.0%;font-weight:normal;font-style:normal;color:#000000;text-decoration:none;text-align:left;vertical-align:bottom;white-space:normal;overflow:hidden;text-indent:0px;padding-left:3px;border-top:1px solid #CCC;border-right:;border-bottom:;} .tblGenFixed td.s3 {background-color:white;font-family:arial,sans,sans-serif;font-size:100.0%;font-weight:normal;font-style:normal;color:#000000;text-decoration:none;text-align:left;vertical-align:bottom;white-space:normal;overflow:hidden;text-indent:0px;padding-left:3px;border-right:;border-bottom:;} </style><body style='border:0px;margin:0px'><table border=0 cellpadding=0 cellspacing=0 id='tblMain'><tr><td><table border=0 cellpadding=0 cellspacing=0 class='tblGenFixed' id='tblMain_0'><tr class='rShim'><td class='rShim' style='width:0;'><td class='rShim' style='width:75px;'><td class='rShim' style='width:169px;'><td class='rShim' style='width:165px;'><td class='rShim' style='width:195px;'><td class='rShim' style='width:248px;'><tr><td class=hd><p style='height:16px;'>.</td><td class='s0'>Feature<td class='s1'>Obj-C+Iphone<td class='s1'>Java+Android<td class='s1'>Phonegap<td class='s1'>Titanium</tr><tr><td class=hd><p style='height:16px;'>.</td><td class='s2'>Language<td class='s3'>Objective C<td class='s3'>Java<td class='s3'>Html+CSS+Javescript<td class='s3'>Javascript</tr><tr><td class=hd><p style='height:16px;'>.</td><td class='s2'>Ide<td class='s3'>Xcode<td class='s3'>Eclipse<td class='s3'>Xcode or any editor<td class='s3'>Any capable JS editor</tr><tr><td class=hd><p style='height:16px;'>.</td><td class='s2'>Can deploy to<td class='s3'>All Ios devices<td class='s3'>All android devices<td class='s3'>Ios and Android devices<td class='s3'>Ios
I recently built the same app with the common mobile technologies, [Obj-C](https://github.com/agiliq/TaxCalculatorIndia), [Android:Java](https://github.com/agiliq/TaxCalculatorAndroid), [Phonegap](https://github.com/agiliq/TaxCalculatorPhonegap), and [Titanium](https://github.com/agiliq/TaxCalculatorTitanium) .


[This is a quick comparison of the app frameworks](https://spreadsheets.google.com/pub?key=0AsTInFQpmXDNdEdJU0ZNNGx3dDA3aXAxV3lXYWhXVHc).


<iframe src="https://spreadsheets.google.com/pub?key=0AsTInFQpmXDNdEdJU0ZNNGx3dDA3aXAxV3lXYWhXVHc&single=true&gid=0&output=html" width="425" height="600px"></iframe>

### Miscellaneous notes

1. Phonegap and Titanium both amazed me. It feels very empowering to code in Html, css and JS, test it in Chrome, debug it with firebug, and deploy to a device without *any* changes. Titanium UI widgets are native, building it with JS was very cool. 
2. Phonegap and Titanium both underwhelmed me. I assumed Phonegap would have widgets which would be 95% of the way to looking like native widgets. It was easy to see the Phonegap UI did not look native. Titanium looks like too much of a black box to me. If you hit a roadblock there did not seem to be good debugging capabilities. The docs also leave a lot to be desired.
3. The tooling for iOs is miles ahead of Android. Editing XML directly is a pain compared to aligning things in Interface Builder.
4. Titanium adds a totally new API over the Andrid/Cocoa API. If I am learning a new API, I would prefer to learn the Official APIs.

Code:
--------

* [Obj-C](https://github.com/agiliq/TaxCalculatorIndia)
* [Android:Java](https://github.com/agiliq/TaxCalculatorAndroid), 
* [Phonegap](https://github.com/agiliq/TaxCalculatorPhonegap)
* [Titanium](https://github.com/agiliq/TaxCalculatorTitanium) 
* [In android marketplace](https://market.android.com/details?id=com.agiliq.taxcalc)
* [In app store](http://itunes.apple.com/us/app/tax-calculator-india/id418307775?mt=8&ls=1)


