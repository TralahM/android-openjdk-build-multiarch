# mobile-openjdk8-build-multiarch

Based on http://openjdk.java.net/projects/mobile/android.html

## Building

### Setup
#### Android
- Download Android NDK r10e from https://developer.android.com/ndk/downloads/older_releases.html and place it in this directory (Can't automatically download because of EULA)
- **Warning**: Do not attempt to build use newer or older NDK, it will lead to compilation errors.

#### iOS
- You should get latest Xcode (tested with Xcode 12).

### Platform and architecture specific environment variables
<table>
      <thead>
        <tr>
          <th></th>
          <th align="center" colspan="7">Environment variables</th>
        </tr>
        <tr>
          <th>Platform - Architecture</th>
          <th align="center">TARGET</th>
          <th align="center">TARGET_JDK</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Android - armv8/aarch64</td>
          <td align="center">aarch64-linux-android</td>
          <td align="center">aarch64</td>
        </tr>
        <tr>
          <td>Android - armv7/aarch32</td>
          <td align="center">arm-linux-androideabi</td>
          <td align="center">arm</td>
        </tr>
        <tr>
          <td>Android - x86/i686</td>
          <td align="center">i686-linux-android</td>
          <td align="center">x86</td>
        </tr>
        <tr>
          <td>Android - x86_64/amd64</td>
          <td align="center">x86_64-linux-android</td>
          <td align="center">x86_64</td>
        </tr>
        <tr>
          <td>iOS/iPadOS - armv8/aarch64</td>
          <td align="center">aarch64-macos-ios</td>
          <td align="center">aarch64</td>
        </tr>
      </tbody>
	</table>

### Run in this directory:

```sh
export BUILD_IOS=1 # only when targeting iOS, default is 0 (target Android)

export BUILD_FREETYPE_VERSION=[2.6.2/.../2.10.4] # default: 2.10.4
export JDK_DEBUG_LEVEL=[release/fastdebug/debug] # default: release
export JVM_VARIANTS=[client/server] # default: client (aarch32), server (other architectures)
export NDK_VERSION=[r25c/.../r10e]
```

#### 1 Setup Target JDK and Target NDK by calling the arch specific script

(`1_ci_build_arch_[aarch32|aarch64|x86|x86_64].sh`) for your desired target.

```sh
### (1_ci_build_arch_[aarch32|aarch64|x86|x86_64].sh)
. 1_ci_build_arch_aarch32.sh # for arm
. 1_ci_build_arch_aarch64.sh
. 1_ci_build_arch_x86.sh # for i686
. 1_ci_build_arch_x86_64.sh
```

#### 2 Download and Setup NDK, run once (Android only)

```sh
. setdevkitpath.sh
wget -nc -nv -O android-ndk-$NDK_VERSION-linux-x86_64.zip "https://dl.google.com/android/repository/android-ndk-$NDK_VERSION-linux.zip"
unzip -q android-ndk-$NDK_VERSION-linux-x86_64.zip
```

#### 3 Get CUPS, Freetype

```sh
./3_getlibs.sh
```

#### 4 Build the libs

```sh
./4_buildlibs.sh
```

#### 5 Clone JDK, run once

```sh
./5_clonejdk.sh
```

#### 6 Configure JDK and build,

If no configuration is changed, run `makejdkwithoutconfigure.sh` instead

```sh
./6_buildjdk.sh
```

#### 7 Remove/Strip JDK debug info

```sh
./7_removejdkdebuginfo.sh
```

#### 8 Pack the built JDK

```sh
./8_tarjdk.sh
```

