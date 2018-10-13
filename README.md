# slirp for macos
=================

## WHY

I have found that no slirp tool can be compiled and running normally on modern macos(such as macOS 10.14). I checked the code finding that the writer of slirp using so many hacks(force 32bit pointer, force structure cast, etc.) that it is hardly to migrate to osx 64bit mode.

But I still want to use it. What should I do?

There is another way to go. Old 32bit programs still can be running on modern macos. But macos 10.14 SDK don't provide i386 SDK, so we can't link the generated objs properly.

The solution is to install old SDK(such as osx 10.7 SDK), then using old SDK compile and link it.
It shows that slirp running very well in i386 mode on macos 10.14.

## HOW

The first step is to install OSX 10.7 SDK. Refer to this repo [devernay/xcodelegacy](https://github.com/devernay/xcodelegacy).

Login into your developer account and download `https://download.developer.apple.com/Developer_Tools/xcode_4.6.3/xcode4630916281a.dmg`.

Then run commands below

```
cd /tmp/
mkdir osx-old-sdk
cd osx-old-sdk
wget https://raw.githubusercontent.com/devernay/xcodelegacy/master/XcodeLegacy.sh
# Move your downloaded dmg here
mv ~/Downloads/xcode4630916281a.dmg .
./XcodeLegacy.sh -osx107 buildpackages
sudo ./XcodeLegacy.sh -osx107 install
sudo mkdir /Developer
sudo ln -sf '/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs' /Developer/SDKs
```

Check `ls /Developer/SDKs` output to verify SDK installed properly.

Then clone this repo and build slirp

```
git clone https://github.com/sharpbai/slirp
cd slirp/src
./configure
make
cp slirp /usr/local/bin/
```

Then you can enjoy it.

