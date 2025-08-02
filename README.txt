####################################################################################################
####################################################################################################

                                      MyCloud

####################################################################################################
####################################################################################################

I would like to explore using REST APIS, I plan to take advantage of google drive API.

Objective of using google api would be to establish a connection and move files from my phone to my

google drive. its an exercise I know there is a google drive app........

To start I am going to look up the documentation for google drive API.

https://developers.google.com/workspace/drive/api/guides/about-sdk

key points:  able to fully access google drive and collaborate with other users from within the app

////////////////////////////////////////////////////////////////////////////////////////////////////

Getting started: add dependencies for google drive via REST API, change entries to version catalog

add resource exclusion for overlapping files in the jars for the google drive dependencies

appears theres a good deal to check out before starting as the google drive api is legacy java

I may use Retrofit or okHttp will need to weigh the pros and cons.

google-api-client-android, --- base client for android but java spice
google-api-services-drive,--- drive specific apis
google-oauth-client-jetty ---google sign in "jetty" <-- outdated for web/desktop


I am getting meta conflicts unless I exclude the new plan is to remove these actions and search

for a more modern solution. I first looked for how to get google drive dependencies and this is

the result now I am going to use

####################################################################################################
circling back for the google drive dependencies after establishing hilt+KSP
####################################################################################################

Authentication -- Google sign in or AppAuth to get OAuth2 access token!!

            // will need to check out the two. and pick
           implementation("com.google.android.gms:play-services-auth:21.0.0") // Google Sign-In
           // OR
           implementation("net.openid:appauth:0.11.1") // AppAuth for lifecycle-safe OAuth2

Choice: I haven't picked yet. easy route seems to be google sign in, AppAuth is a bit more complex
Reason:

Networking -- Retrofit or OkHttp "hit Drive REST endpoints directly"

            // they work together actually OkHttp powers Retrofit

            implementation("com.squareup.retrofit2:retrofit:2.9.0")
            implementation("com.squareup.retrofit2:converter-gson:2.9.0")
            implementation("com.squareup.okhttp3:okhttp:4.12.0")

Choice: both will be used.
Reason: RetroFit should handle the majority of the work for me, and OkHttp will be used to solve
the any problems Retrofit is not handling.

Coroutines + Json -- for async and serialization

            implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3")

            implementation("com.google.code.gson:gson:2.10.1")
            //or
            implementation("com.squareup.moshi:moshi:1.15.0")

####################################################################################################
If choosing Moshi --  ksp("com.squareup.moshi:moshi-kotlin-codegen:1.15.0")
####################################################################################################
I just also looked back into Dependencies injection that's for sure going to be a thing again from

the start. I plan again Hilt with KSP.                                                                  Gradle commands: ./gradlew clean,build,projects, help --scan

took a good amount of work to get KSP working, dependencies can be a real challenge.

once I figured out the correct dependencies to declare it came together. I also added the room ksp.

just in case I want a local database again in this project. Who knows I may practice syncing local

data with remote. Might come in handy on the previous app as well. app was having a strange crash

after dependencies updates I ran

.\gradlew :mycloud:dependencyInsight --configuration releaseRuntimeClasspath --dependency androidx.work

*above command provide gradle dependencies insight.

the problem is something to do with the android.work:work-runtime:2.3.4 which predates Android 12's

FLAG_IMMUTABLE enforcement. which was casing the problem.  Solution is to provide the correct

version in the libs.versions.toml file. tomul or tom L.

update this in fact did fix the issue. the app is on its way.

####################################################################################################