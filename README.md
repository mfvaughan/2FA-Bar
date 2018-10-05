# 2FA-Bar
Walkthrough on how to enable displaying time-based one-time passwords (that are usually generated and copied through authenticator apps) on MacOSX toolbar.


## Initial Setup:
1. Install python (I'm using python3.6) and the pyotp package (https://github.com/pyotp/pyotp).

2. Download the free BitBar app (https://github.com/matryer/bitbar).

3. Download a barcode scanning app with MFA secret decoding capabilities (the generically-named "Barcode Scanner" Android app worked great for me),

4. Identify the 2FA/MFA one-time password you need to step up (for example Google). You will need access to the original scannable QR code you use to set up your authenticator app, so you will likely have to delete and re-configure with a new QR code.

5. Set up your 2FA/MFA and get to the "scan QR code" step - now use the Barcode Scanner app to scan the authenticator QR code 

6. After scaning, take special note of the "secret" at the end of the URI starting with "otpauth://" (e.g., "totp/alice@google.com?secret=asdf2135"). In this example, the secret would be "asdf1235", and this is what you will need to store in a separate credentials file in a safe place.


## BitBar Setup
1. Install BitBar (linked above) and create a new empty Plugins folder.

2. Create a python file "otp.py" with the following code:
```python
import pyotp
from credentials import auth_secret

# You can add as many otp secrets and print statements as you want here ...
otp = pyotp.TOTP(auth_secret)
print("OTP:", otp.now())
```

3. Create a bash script file with the name "otp.30s.sh" with the following code and save it in the BitBar Plugins folder you created earlier. 

Note: The "30s" in the filename is the refresh rate for running the bash script - you can edit this to your preference (e.g., 1d, 2h, 5m, 30s, etc.). I'm using 30 seconds here, because that is the standard refresh rate of the time-based one-time password algorithm

``` bash
#!/bin/bash

/path/to/python3.6 /path/to/python/code/otp.py

```

4. Launch BitBar and if all was configured correctly using the above steps, you should see your 6-digit one-time password displayed in the MacOSX toolbar (you may have to select the icon, and click Preferences > Refresh All).

### And that's it! 

If you used the correct secret from the QR code, you should now get an up-to-date OTP displayed in the toolbar every 30 seconds. You can even test it out by matching it up with your authenticator app! I think this is a pretty cool time-saver for anyone who makes use of 2FA often, or needs to automate 2FA during testing.
