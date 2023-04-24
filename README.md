# About this demo

This app demo has a propose to demonstrate how to implement the oauth authentication in Android apps, using Jitpack Navigation Components.


**IMPORTANT:**

`Chrome Custom Tabs` has no method to close itself, and this implementation is based on redirecting to previous Activity, by Intent flags, finishing the called Activity with registered scheme.


## Introduction

These are some example how to implement a sign-in on OAuth flow to use Trustly JavaScript SDK.
The code is using Kotlin language implementation.

### CustomWebView

It is a simple custom view with a WebView inside, to open the transported url.
In your custom web view you need to create a CustomTabIntent to open the url:

```kotlin
    private fun launchUrl(context: Context, url: String) {
        val customTabsIntent = CustomTabsIntent.Builder().build()
        customTabsIntent.launchUrl(context, Uri.parse(url))
    }
```

### TrustlyRedirectActivity

When the application receive some action for `redirect-navigation`, or the name that you defined in `urlScheme`, it will call your target Activity with some flags, and reload it.

```kotlin
    Intent(this, MainActivity::class.java).apply {
        addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP or Intent.FLAG_ACTIVITY_SINGLE_TOP)
    }.run { startActivity(this) }
    finish()
```

### AndroidManifest

```xml
    <activity
    android:name=".TrustlyRedirectActivity"
    android:exported="true">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="redirect-navigation" />
            <data android:path="/second-fragment" />
        </intent-filter>
    </activity>
```

### ProceedToChooseAccount

Finally, in order to support a smooth user experience when an OAuth login authorization is completed and the user returns to the Lightbox, call this function using some code like this:

```kotlin
    webView.loadUrl("javascript:window.Trustly.proceedToChooseAccount();")
```