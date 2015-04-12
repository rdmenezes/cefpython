# `DpiAware` class #

Available only on Windows. All methods of this class are static, access them through [cefpython](cefpython.md).`WindowUtils`.

Example usage of High DPI support is in the wxpython.py example.

## Introduction ##

> By default if DPI awareness is not enabled in application, then OS performs display scaling. That causes text to look fuzzy on high DPI displays. The solution is to enable DPI awareness and use the ApplicationSettings.`auto_zooming` option. High DPI support is available only on Windows.

> Enabling High DPI support in app can be done by embedding a DPI awareness xml manifest in executable (see [Issue 112](https://code.google.com/p/cefpython/issues/detail?id=112) comment #2), or by calling the `DpiAware.SetProcessDpiAware` method. Embedding xml manifest is the most reliable method. There is also an another way by writing registry key, but we won't discuss this method here.

> The downside of calling `SetProcessDpiAware` is that scrollbar in CEF browser is very small. This is because DPI awareness was set too late, after executable and the CEF dll was loaded. To fix that embed DPI awareness xml manifest in the .exe file.

> Additionally you have to set the ApplicationSettings.`auto_zomming` option to "system\_dpi". This will cause browser content to be zoomed automatically using system DPI settings. On Win7 these can be set in: Control Panel > Appearance and Personalization > Display.

> When cefpyhon detects that application is DPI aware, it will automatically set ApplicationSettings."auto\_zooming" to "system\_dpi". User can manually enable DPI awareness in application by going to .exe properties > Compatibility tab > and checking the box labeled "Disable display scaling on high DPI settings". If you wish to not enable auto zooming in such case then set "auto\_zooming" to an empty string.

### Screenshot ###

> This screenshot shows the difference between native OS display scaling and High DPI support.  Tested on a laptop with 1920x1080 resolution with Win7 DPI settings being set to "Larger 150%". The window on the left is using OS display scaling. The window on the right has enabled DPI awareness and "auto\_zooming" option set to "system\_dpi".

![http://cefpython.googlecode.com/git/cefpython/var/dpi-aware.png](http://cefpython.googlecode.com/git/cefpython/var/dpi-aware.png)

## Static methods ##


tuple **CalculateWindowSize**(int width, int height)

> This utility function will adjust width/height using
> OS DPI settings. For 800/600 with Win7 DPI settings
> being set to "Larger 150%" will return 1200/900.

> Calculation for DPI < 96 is not yet supported. Use
> the `GetSystemDpi` method for that.



tuple **GetSystemDpi**():

> Returns tuple(int dpix, int dpiy).

> Win7 DPI (Control Panel > Appearance and Personalization > Display):

  * text size Larger 150% => dpix/dpiy 144
  * text size Medium 125% => dpix/dpiy 120
  * text size Smaller 100% => dpix/dpiy 96

> Example zoom levels based on DPI. For use with the ApplicationSettings.`auto_zooming` option.

  * dpix=96 zoomlevel=0.0
  * dpix=120 zoomlevel=1.0
  * dpix=144 zoomlevel=2.0
  * dpix=72 zoomlevel=-1.0

> If DPI awareness wasn't yet enabled, then `GetSystemDpi` will always return a default 96 DPI.

bool **IsProcessDpiAware**()

> To check whether OS display scaling on high DPI settings was disabled. DPI awareness may be enabled by calling `SetProcessDpiAware`. It may also be enabled manually by user by changing options in .exe properties > Compatibility tab.

> On Win8 this will return True if DPI awareness is set to either "System DPI aware" or "Per monitor DPI aware".

void **SetProcessDpiAware**()

> Enables DPI awareness for the running process. Embedding a DPI manifest in .exe is the prefered way, as it gives more reliable results, otherwise some display bugs may appear (discussed in the "Introduction" section on this page).