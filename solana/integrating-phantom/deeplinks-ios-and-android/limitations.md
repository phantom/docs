# Limitations

### Android

Android has a 500kb Transaction limit when passing data between services and apps. The requesting app may crash with a `TransactionTooLarge` exception when requesting a string >500kb (over 31k characters). This tends to happen with significantly large intents.

### iOS

iOS is not known to have a 500kb transaction and allows [transmissions up to 1 MB](https://github.com/zoul/ios-url-scheme-length-limit/blob/master/Transmitter/ViewController.swift).
