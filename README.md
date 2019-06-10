# FingerprintIdentify

**1. Gradle** 

    compile 'com.wei.android.lib:fingerprintidentify:1.2.6'

**2. AndroidManifest**

    <uses-permission android:name="android.permission.USE_FINGERPRINT"/>
    <uses-permission android:name="com.fingerprints.service.ACCESS_FINGERPRINT_MANAGER"/>
    <uses-permission android:name="com.samsung.android.providers.context.permission.WRITE_USE_APP_FEATURE_SURVEY"/>

**3. FingerprintIdentify api**

    mFingerprintIdentify = new FingerprintIdentify(this);       // create object
    mFingerprintIdentify.setSupportAndroidL(true);              // support android L
    mFingerprintIdentify.setExceptionListener(listener);        // exception callback for develop
    mFingerprintIdentify.init();                                // init

    mFingerprintIdentify.isFingerprintEnable();                 // is fingerprint usable
    mFingerprintIdentify.isHardwareEnable();                    // is hardware usable
    mFingerprintIdentify.isRegisteredFingerprint();             // is fingerprint collected
    mFingerprintIdentify.startIdentify(maxTimes, listener);     // start identify
    mFingerprintIdentify.cancelIdentify();                      // cancel identify
    mFingerprintIdentify.resumeIdentify();                      // resume identify

**4. startIdentify method**

    mFingerprintIdentify.startIdentify(3, new BaseFingerprint.IdentifyListener() {
        @Override
        public void onSucceed() {
            // succeed, release hardware automatically
        }

        @Override
        public void onNotMatch(int availableTimes) {
            // not match, try again automatically
        }

        @Override
        public void onFailed(boolean isDeviceLocked) {
            // failed, release hardware automatically
            // isDeviceLocked: is device locked temporarily
        }

        @Override
        public void onStartFailedByDeviceLocked() {
            // the first start failed because the device was locked temporarily
        }
    });

**5. Proguard**

    -ignorewarnings

    # Fingerprint
    -keep class com.fingerprints.service.** { *; }
    
    # SmsungFingerprint
    -keep class com.samsung.android.sdk.** { *; }

**6. Notice**

    1. Usually, device will be locked temporarily when read incorrect fingerprints continuously 5 times.
       And it need to takes about 30 seconds to get back to normal.
       But it's not standardized, SDK has no limit to this.

    2. About 'com.android.support:appcompat-v7:25.3.1' version
       The class FingerprintManagerCompatApi23 will check the feature FEATURE_FINGERPRINT from version 25.
       More info：https://code.google.com/p/android/issues/detail?id=231939

    3. Some manufacturers will transplant standard fingerprint API to the device
       which version less than Android M, such as OPPO.
       But the API will be modified sometimes and cause calling crash, such as VIVO.

    4. We need to check the manufacturers because sdk is available on some other devices.

    5. SDK runs abnormally on Note3 sometimes, it can't switch to mback mode event called release。
