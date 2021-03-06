# sudo-touchid
`sudo-touchid` is a fork of `sudo` with Touch ID support on macOS (powered by the `LocalAuthentication` framework). Once compiled, it will allow you to authenticate `sudo` commands with Touch ID in the Terminal on supported Macs (such as the late 2016 MacBook Pros).

Since Darwin sources for macOS 10.12 are not available yet, this project is based on `sudo` sources corresponding to OS X 10.11.6 and obtained from [opensource.apple.com](http://opensource.apple.com).

## Warnings

Please note:

- This version of `sudo` is based on OS X 10.11.6 sources. I am not sure if enough has changed in macOS 10.12 to cause any malfunctions.
- I am not a security expert. While I am using this as a fun experiment on my personal computer, your security needs may vary.   

## Building

To build `sudo-touchid`, simply open the included Xcode project file with Xcode 8+, select the `Build All` target, and click **Build**.

## Running

If we try running our newly-built `sudo` executable now, we'll get an error:

> sudo must be owned by uid 0 and have the setuid bit set

To fix this, we can use our system's `sudo` command and the `chown/chmod` commands to give our newly-built `sudo` the permissions it needs:

> cd (built-products-directory)

> sudo chown root:wheel sudo && sudo chmod 4755 sudo

Now if we try running our copy of `sudo`, it should work:

> cd (built-products-directory)

> ./sudo -s

If you don't have a Mac with a biometric sensor, `sudo-touchid` will fall back to the regular password prompt. If you'd still like to test whether the `LocalAuthentication` framework is working correctly, you can change the `kAuthPolicy` constant from `LAPolicyDeviceOwnerAuthenticationWithBiometrics` to `LAPolicyDeviceOwnerAuthentication` in the code. This will present a dialog box asking the user for his or her password:

<img src="https://github.com/mattrajca/sudo-touchid/blob/master/images/auto_fallback.png?raw=true" width=556 height=301 />

While not useful in practice, you can use this to verify that the `LocalAuthentication` code does in fact work.

## Installing

Replacing the system's `sudo` program is quite risky (can prevent your Mac from booting) and requires disabling System Integrity Protection (aka "Rootless").

Instead of replacing `sudo`, we can install it under a different name to `/usr/local/bin`. I chose to use `suto`, where the `t` stands for Touch ID. 🙃

> sudo cp (built-products-directory)/sudo /usr/local/bin/suto

> sudo chown root:wheel /usr/local/bin/suto && sudo chmod 4755 /usr/local/bin/suto

Now you should be able to run `suto` in any Terminal (or iTerm) window with Touch ID support!
