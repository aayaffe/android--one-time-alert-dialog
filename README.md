# OneTimeAlertDialog
Android AlertDialog that only shows once for a given string.

This is a subclass of `AlertDialog` with all its features supported. The only difference is that a key is provided in its initialization, which is used to check the default SharedPreferences and save itself once shown.



## Use cases

- Show your app's recent updates prompt just once per version name.
- Show a "rate now" prompt just once.
- Quickly create a randomly shown message without worrying about the preferences.



## Usage
First, copy `OneTimeAlertDialog.java` to your project. Then, do something similar to the following.

### Simple case

    OneTimeAlertDialog.Builder(this, "my_dialog_key")
            .setTitle("My Title")
            .setMessage("My Message")
            .show();

### Show recent updates prompt just once per version name

    /** Show the recent updates prompt once per version. */
    public static void showRecentUpdateOnce(Activity activity) {
        OneTimeAlertDialog.Builder(activity, "recent_updates_dialog" + BuildConfig.VERSION_NAME)
                .setTitle(activity.getString(R.string.recent_updates_title))
                .setMessage(activity.getString(R.string.recent_updates_message))
                .show();
    }

### Show a "rate now" prompt just once

    public static void showRateDialogOnce(final Activity activity) {
        OneTimeAlertDialog.Builder(activity, "rate_dialog")
                .setTitle("One-time message")
                .setMessage("If you like me, rate me. ;)")
                .setPositiveButton("Rate", new DialogInterface.OnClickListener() {
                    public void onClick(DialogInterface dialog, int whichButton) {
                        Uri uri = Uri.parse("market://details?id=" + activity.getPackageName());
                        activity.startActivity(new Intent(Intent.ACTION_VIEW, uri));
                    }
                })
                .setNegativeButton("Not now", null)
                .show();
    }



## Limitations

- `show()` always returns null (rather than the AlertDialog).
- Do **NOT** directly call `create()` for OneTimeAlertDialog.Builder. Otherwise a regular AlertDialog without a prefsKey will be returned, thus show() wouldn't be limited.

These two (small) limitations were allowed as a tradeoff to keep the code vastly simplified, smaller, and more future-proof. These two issues are from extending `AlertDialog.Builder` for `OneTimeAlertDialog.Builder`. So, calling `create()` creates a regular AlertDialog instead of a OneTimeAlertDialog. And, show() calls create() internally, so it would only return a regular AlertDialog also. So, it was chosen to always return null, especially since I've never reused the AlertDialog for the one-time prompts. One way around these limitations is to recreate the entire AlertDialog.Builder (basically a large copy+paste), but I didn't find that worth the tradeoffs.

Having said that, if you know a simple way to hide these limitations from the public API, then I'd be happy to learn and fix it!



## License

	/**
	 * The MIT License (MIT)
	 *
	 * Copyright (c) 2015 Danial Goodwin
	 *
	 * Permission is hereby granted, free of charge, to any person obtaining a copy
	 * of this software and associated documentation files (the "Software"), to deal
	 * in the Software without restriction, including without limitation the rights
	 * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
	 * copies of the Software, and to permit persons to whom the Software is
	 * furnished to do so, subject to the following conditions:
	 *
	 * The above copyright notice and this permission notice shall be included in all
	 * copies or substantial portions of the Software.
	 *
	 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
	 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
	 * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
	 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
	 * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
	 * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
	 * SOFTWARE.
	 */