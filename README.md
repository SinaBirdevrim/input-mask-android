<img src="https://raw.githubusercontent.com/RedMadRobot/input-mask-ios/assets/Assets/input-mask-cursor.gif" alt="Input Mask" height="40" />

[![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)
[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-Input%20Mask-brightgreen.svg?style=flat)](https://android-arsenal.com/details/1/4642)
[![Bintray](https://api.bintray.com/packages/redmadrobot-opensource/android/input-mask-android/images/download.svg)](https://bintray.com/redmadrobot-opensource/android/input-mask-android/_latestVersion)
[![codebeat badge](https://codebeat.co/badges/e87a117d-3be1-407b-ad4c-973f90d88cd2)](https://codebeat.co/projects/github-com-redmadrobot-input-mask-android-master)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg)]()

[![Platform](https://cdn.rawgit.com/RedMadRobot/input-mask-ios/assets/Assets/shields/platform.svg)]()[![Android](https://cdn.rawgit.com/RedMadRobot/input-mask-ios/assets/Assets/shields/android.svg)](https://github.com/RedMadRobot/input-mask-android)[![iOS](https://cdn.rawgit.com/RedMadRobot/input-mask-ios/assets/Assets/shields/ios_rect.svg)](https://github.com/RedMadRobot/input-mask-ios)[![macOS](https://cdn.rawgit.com/RedMadRobot/input-mask-ios/assets/Assets/shields/macos.svg)](https://github.com/RedMadRobot/input-mask-ios)

<img src="https://github.com/RedMadRobot/input-mask-android/blob/assets/assets/gif-animations/direct-input.gif" alt="Direct input" width="210"/>
<details>
<summary>More GIFs [~3 MB]</summary>
  <img src="https://github.com/RedMadRobot/input-mask-android/blob/assets/assets/gif-animations/making-corrections.gif" alt="Direct input" width="210"/>
  <img src="https://github.com/RedMadRobot/input-mask-android/blob/assets/assets/gif-animations/cursor-movement.gif" alt="Direct input" width="210"/>
  <img src="https://github.com/RedMadRobot/input-mask-android/blob/assets/assets/gif-animations/do-it-yourself.gif" alt="Direct input" width="210"/><br/>
  <img src="https://github.com/RedMadRobot/input-mask-android/blob/assets/assets/gif-animations/complete.gif" alt="Direct input" width="210"/>
  <img src="https://github.com/RedMadRobot/input-mask-android/blob/assets/assets/gif-animations/extract-value.gif" alt="Direct input" width="210"/>
</details>

## Migration to `4.3.x`
	
Make sure you've updated your dependencies, as we are moving from the old
```gradle
com.redmadrobot:inputmask:4.1.0
```
to
```gradle
com.redmadrobot:input-mask-android:4.3.1
```

## Description

`Input Mask` is an [Android](https://github.com/RedMadRobot/input-mask-android) & [iOS](https://github.com/RedMadRobot/input-mask-ios) native library allowing to format user input on the fly.

The library provides you with a text field listener; when attached, it puts separators into the text while user types it in, and gets rid of unwanted symbols, all according to custom predefined pattern.

This allows to reformat whole strings pasted from the clipboard, e.g. turning pasted `8 800 123-45-67` into  
`8 (800) 123 45 67`.

Each pattern allows to extract valuable symbols from the entered text, returning you the immediate result with the text field listener's callback when the text changes. Such that, you'll be able to extract `1234567` from `8 (800) 123 45 67` or `19991234567` from `1 (999) 123 45 67` with two different patterns.

All separators and valuable symbol placeholders have their own syntax. We call such patterns "masks".

Mask examples:

1. International phone numbers: `+1 ([000]) [000] [00] [00]`
2. Local phone numbers: `([000]) [000]-[00]-[00]`
3. Names: `[A][-----------------------------------------------------]` 
4. Text: `[A…]`
5. Dates: `[00]{.}[00]{.}[9900]`
6. Serial numbers: `[AA]-[00000099]`
7. IPv4: `[099]{.}[099]{.}[099]{.}[099]`
8. Visa card numbers: `[0000] [0000] [0000] [0000]`
9. MM/YY: `[00]{/}[00]`

## Questions & Issues

Check out our [wiki](https://github.com/RedMadRobot/input-mask-android/wiki) for further reading.  
Please also take a closer look at our [Known issues](#knownissues) section before you incorporate our library into your project.

For your bugreports and feature requests please file new issues as usually.

Should you have any questions, search for closed [issues](https://github.com/RedMadRobot/input-mask-android/issues?q=is%3Aclosed) or open new ones at **[StackOverflow](https://stackoverflow.com/questions/tagged/input-mask)** with the `input-mask` tag.

We also have a community-driven [cookbook](https://github.com/RedMadRobot/input-mask-android/blob/master/Documentation/COOKBOOK.md) of recipes, be sure to check it out, too.

<a name="installation" />

## Installation

### Gradle

Make sure you've added Kotlin support to your project.

```gradle
repositories {
    jcenter()
}

dependencies {
    implementation 'com.redmadrobot:input-mask-android:4.3.1'
    
    implementation 'org.jetbrains.kotlin:kotlin-stdlib:$latest_version'
}
```

<a name="knownissues" />

# Known issues
## InputMask vs. `NoClassDefFoundError`

```
java.lang.NoClassDefFoundError: Failed resolution of: Lkotlin/jvm/internal/Intrinsics;
```
Receiving this error might mean you haven't configured Kotlin for your Java only project. Consider explicitly adding the following to the list of your project dependencies:
```
implementation 'org.jetbrains.kotlin:kotlin-stdlib:$latest_version'
```
— where `latest_version` is the current version of `kotlin-stdlib`.

## InputMask vs. `android:inputType` and `IndexOutOfBoundsException`

Be careful when specifying field's `android:inputType`. 
The library uses native `Editable` variable received on `afterTextChange` event in order to replace text efficiently. Because of that, field's `inputType` is actually considered when the library is trying to mutate the text. 

For instance, having a field with `android:inputType="numeric"`, you cannot put spaces and dashes into the mentioned `Editable` variable by default. Doing so will cause an out of range exception when the `MaskedTextChangedListener` will try to reposition the cursor.

Still, you may use a workaround by putting the `android:digits` value beside your `android:inputType`; there, you should specify all the acceptable symbols:
```xml
<EditText
    android:inputType="number"
    android:digits="0123456789 -."
    ... />
```
— such that, you'll have the SDK satisfied.

Alternatively, if you are using a programmatic approach without XML files, you may consider configuring a `KeyListener` like this:
```java
editText.setInputType(InputType.TYPE_CLASS_NUMBER);
editText.setKeyListener(DigitsKeyListener.getInstance("0123456789 -.")); // modify character set for your case, e.g. add "+()"
```

## InputMask vs. autocorrection & prediction
> (presumably fixed by [PR50](https://github.com/RedMadRobot/input-mask-android/pull/50))

Symptoms: 
* You've got a wildcard template like `[________]`, allowing user to write any kind of symbols;
* Cursor jumps to the beginning of the line or to some random position while user input.

In this case text autocorrection & prediction might be a root cause of your problem, as it behaves somewhat weirdly in case when field listener tries to change the text during user input.

If so, consider disabling text suggestions by using corresponding input type:
```xml
<EditText
    ...
    android:inputType="textNoSuggestions" />
```
Additionally be aware that some of the third-party keyboards ignore `textNoSuggestions` setting; the recommendation is to use an extra workaround by setting the `inputType` to `textVisiblePassword`.

## InputMask vs. `android:textAllCaps`
> Kudos to [Weiyi Li](https://github.com/li2) for [reporting](https://github.com/RedMadRobot/input-mask-android/issues/85) this issue

Please be advised that `android:textAllCaps` is [not meant](https://developer.android.com/reference/android/widget/TextView.html#setAllCaps(boolean)) to work with `EditText` instances:

> This setting will be ignored if this field is editable or selectable.

Enabling this setting on editable and/or selectable fields leads to weird and unpredictable behaviour and sometimes even [crashes](https://twitter.com/dimsuz/status/731117910337441793). Instead, consider using `android:inputType="textCapCharacters"` or workaround by adding an `InputFilter`:

```java
final InputFilter[] filters = { new InputFilter.AllCaps() };
editText.setFilters(filters);
```

Bare in mind, you might have to befriend this solution with your existing `android:digits` [property](#inputmask-vs-androidinputtype-and-indexoutofboundsexception) in case your text field accepts both digits and letters. 

## References

The list of projects that are using this library which were kind enough to share that information.

Feel free to add yours below.

## Special thanks

These folks rock:

* Artem [Fi5t](https://github.com/Fi5t) Kulakov
* Nikita [nbarishok](https://github.com/nbarishok) Barishok
* Roman [yatsinar](https://github.com/yatsinar) Iatcyna
* Alexander [xanderblinov](https://github.com/xanderblinov) Blinov
* Vladislav [Shipaaaa](https://github.com/Shipaaaa) Shipugin
* Vadim [vkotovv](https://github.com/vkotovv) Kotov

# License

The library is distributed under the MIT [LICENSE](https://opensource.org/licenses/MIT).
