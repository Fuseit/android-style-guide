# Deploying for Android
----------
At the time or writing we do not have fully automated deployment. We do however have semi auto deployment via fastlane and manual deployment. 
If you need to deploy to production by promoting the alpha or beta channels then process to the [Post Deployment](#43-post-deployment) section
*Note: All the different steps below will automatically increment the build and version numbers. This document also assumes that an initial deployment has been made as this require more steps than what are listed here.

# Table of contents
- [Deploying via Fastlane](#1-deploying-via-fastlane)
- [Deploying via Android Studio](#2-deploying-via-android-studio)
- [Deploying via React Native](#3-deploying-via-react-native)
- [Deploy APK via Google Play Store](#4-deploy-apk-via-google-play-store)

## 1. Deploying via Fastlane
This is by far the easiest way to deploy but can be error prone. After following the steps below and getting an unsuccessful deployment then resort to one of the other method of deploying.

#### 1.1 Prerequisite
* If this is the first time deploying via any means you will need to set up the keystore and passwords by performing steps 3 to 9 from [Deploying via Android Studio](#2-deploying-via-android-studio)

### 1.2 Procedure
1. Check out the beta branch locally
2. Using your terminal navigate to the FuseMobile folder
3. Enter one of the following commands depending where you are releasing to
3.1.‘fastlane android beta’ This will deploy the app to Fuse beta stage in the play store
3.2 ‘fastlane android princestrust_beta’ This will deploy the app to Prince’s Trust beta stage in the play store
4. If you encounter any error during the deployment phase make sure to discard any changes made to the project. This will most likely only be version bumping. 
5. You will get a fastlane message in the #mobile-general if everything is successful. Refer to the troubleshooting section below if you encounter repeating issues. Feel free to amend this section on solving

#### 1.3 Troubleshooting:
* Sometimes the build will just fail and simply rerunning the deployment process will work, however there are times when some issue can trigger a deployment error.
* You have a not checked out the beta branch locally
* You have not set up the keystore: Refer to steps 3 to 9 from [Deploying via Android Studio](#2-deploying-via-android-studio)
* Missing *node_module* packages
* Delete the *react-native-windows* package from *node_modules*

## 2. Deploying via Android Studio
*Use this as a last resort if fastlane is not working.*

1. Check out the beta branch locally
2. Open the project in Android studio
3. Once everything has synced and gradle has finished its processes, 
3.1 select build from the top menu
3.2 Select “Generate Signed APK”
4. The generate signed apk screen should have popped up
5. Ensure ‘app’ is selected in the module drop down
6. Press next
7. Navigate to the keystore path. Not everyone has access to this as it is a security key for the Android app. At the time of writing this Roan and Gonzalo have the keystore.
8. Key Store Password and Key Password are the same. These are located in the *gradle.properties* file.
9. For the Key Alias type *key_alias*
10. Press next
11. Select the destination where you want the apk file to be stored (ie desktop)
12. Ensure the build type is set to release
13. In the flavours section select the one which you are releasing too.
13.1 For Fuse select *dev*
13.2 For Prince’s Trust select *princestrust*
14. Press finish which will kick off the building process. This might take a few minutes
15. Once finished a popup will be displayed that gives you the option to “reveal in finder”. If not check the status at the bottom of Android Studio which should say “Generate Signed APK generated successfully”. If the pop up has disappeared before you could click it you can click on the status bar to open the events console where you will be able to select “Reveal in finder” again or simply navigate to where you set the output destination to.
16. Refer to the [Deploying via Google Play Store](#4-deploying-via-google-play-store) section to continue the process
17. Once successfully deployed you will need to commit the file where the build and version numbers were changed into beta. This is important as the next deployment will use these number to again increment them.

## 3. Deploying via React Native
#### 3.1 Prerequisite: 
* If this is the first time deploying via any means you will need to set up the keystore and passwords by performing steps 3 to 9 from [Deploying via Android Studio](#2-deploying-via-android-studio).
* Use the terminal for the steps in the procedure section

### 3.2 Procedure
1. Check out the beta branch locally.
2. Navigate to the project’s android folder:
2.1 ../FuseMobile/android/
3. Enter the following command depending on the flavour you want:
3.1 ./gradlew assembleDevRelease
3.2 ./gradlew assemblePrincestrustRelease
4. Once the process is finished the apk will be stored in the following location
4.1 FuseMobile/android/app/build/outputs/apk
5. The apk you want to deploy should look something like this:
5.1 app-dev-release.apk or app-princestrust-release.apk depending on the flavour you chose. A key point is to ensure the word ‘release’ appears in the name, if not then you will need to restart the process.
6. Continue with the deployment process by following the [Deploying via Google Play Store](#4-deploying-via-google-play-store) process.
7. Once successfully deployed you will need to commit the file where the build and version numbers were changed into beta. This is important as the next deployment will use these number to again increment them.

## 4. Deploy APK via Google Play Store
### 4.1 Prerequisite
* You will need an apk file to upload. Refer to [Deploying via Android Studio](#2-deploying-via-android-studio) or [Deploying via React Native (Terminal)](#3-deploying-via-react-native(terminal)) 
* You will need login details with the appropriate access in order to deploy via the play store
### 4.2 Proceedure
1. Log in using your credentials
2. From the list of application select the one you want to deploy too
3. From the menu on the left click “Release Management” then click “App Release”
3.1 You will be presented with 3 options (Production, Beta and Alpha). You should hardly, if ever be releasing directly to production. The ideal flow is to promote beta and alpha version until they make it to production.
4. Click “Manage Alpha”
5. Click “Create Release”
6. Either navigate to the apk file or drag and drop it into the window that pops up and Click “Save Draft” after processing has been completed
7. Complete the what’s new’ section and click “Save” then “Review”
8. Click the “Start Roll-out to” button to finish the deployment process

### 4.3 Post Deployment
The follow section is only for promoting either the alpha or beta versions of the app to production. While the previous methods can also used to deploy to production. Ideally we only want to promote a version of the app to production once it has been in either the alpha or beta channels and QA has signed off.

1. Repeat steps 1 to 4 in the [Deploying via Google Play Store](#4-deploying-via-google-play-store) section, but instead of clicking “Manage Alpha” click either “Manage Alpha” or  “Manage Beta” depending on which channel you want to promote.
2. Select one of the following options “Release to Production” or  “Release to (Beta or Alpha)”. 
3. Repeat steps 7 and 8 in the [Deploying via Google Play Store](#4-deploying-via-google-play-store) section to complete the promotion process

