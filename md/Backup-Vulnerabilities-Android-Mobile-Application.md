## Backup Vulnerabilities Android Mobile Application

hello everyone today I will share about Android Backup Vulnerabilities, this is one of the findings that I often find when doing pentest on android applications, especially on mobile applications. I made this article because it was quite inspired by [this blog](https://vishwarajbhattrai.wordpress.com/2017/07/17/finding-backup-vulnerabilities-in-android-apps/) Now lets just go straight to the discussion.

Android Backup Vulnerabilities are vulnerabilities where an application allows backups for the application, this vulnerability will be very impactful if the same application has Insecure data storage vulnerabilities, because the application’s internal files can be stolen without root, even though the android device must turn on usb debugging

### How to look for it?
Check the androidmanifest.xml file and highlight the android:allowBackup text, if android:allowBackup = "true" this means that the application is vulnerable to android backup vulnerabilities, and vice versa if the value is false, it means that the android application is not vulnerable to Android Backup Vulnerabilities, it's easy, right? okay, continue to the demo.

Unpack the app using jadx and then open the Androidmanifest.xml file, this file is usually found in Resource

![Preview 1](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*d0Gij60zBznjCu_vbdsJVQ.png)

Using the adb (android debug brigde) tool, we try to backup the application folder with commands more or less like this, and don’t forget to connect the adb via usb or wireless.

```bash
adb backup -f <backup-name.ab> <app-package-name>
adb backup -f application com.app.info
```

Using [ABE (Android Backup Extractor)](https://github.com/nelenkov/android-backup-extractor/releases/download/master-20221109063121-8fdfc5e/abe.jar) convert .ab files to .tar so that they can be extracted, look like the picture below the internal application folder can be retrieved. When converting the abe tool, it will ask for the password that we created before

![Preview 2](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*0a_hc7NYP57p3np_BYQ6_g.png)

![Preview 3](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*ERbUPQqqOyK69dPIR67Zfw.png)

### Refrence
* [https://owasp.org/www-project-mobile-top-10/2023-risks/m9-insecure-data-storage](https://owasp.org/www-project-mobile-top-10/2023-risks/m9-insecure-data-storage)

* [https://vishwarajbhattrai.wordpress.com/2017/07/17/finding-backup-vulnerabilities-in-android-apps/](https://vishwarajbhattrai.wordpress.com/2017/07/17/finding-backup-vulnerabilities-in-android-apps/)

* [https://www.appsealing.com/insecure-data-storage/](https://www.appsealing.com/insecure-data-storage/)
