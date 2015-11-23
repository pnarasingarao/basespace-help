---
layout: article
title: The iOS App for BaseSpace
hide_welcome_banner: true
---

The iOS app for BaseSpace makes it easier for you to browse through your Runs and Analyses in BaseSpace.  The app will let you review metrics, charts, and other information about Runs and Analyses.  This article will cover the features available in the current version of the app.  The BaseSpace iOS app is available in the Apple App Store [here(link to store)](), try it out for yourself!

##Log in to the iOS App

When you first launch the app, you will be asked to log in using your BaseSpace credentials.  Once you have logged in, you will be taken directly to the Runs list page.

If you would like to log in as a different user, you will have to first log out of the app.  To do so, please follow the directions below.

##Log out of the iOS App

At some point, you may want to log out of the BaseSpace iOS app and even log into a different account.  In order to do this, simply: 

* Click on the **Settings** icon ![Settings Icon](/images/mobile-ios/settings.png) in the top right corner of the app, then
* Click on the large red **Log out** button to log out of your BaseSpace account in the app

##Turn push notifications on/off

The iOS app for BaseSpace comes preconfigured with push notifications enabled.  In some cases, you may wish to turn off push notifications for the application.  In order to turn off push notifications for the app:

* Go to the **Settings** on your device
* Select **Notifications**
* Toggle off push notifications for this app 

##View your list of Runs

To view the list of Runs in your account and to monitor overall status, click on the **Runs** sequencer icon ![Runs List Icon](/images/mobile-ios/runs-unselected.png).  You can click on any Run in the list in order to view more details about that particular Run.

This page includes:

* Run Name - The name of the Run that the user specified in the SampleSheet or in the Prep Tab (NextSeq)
* Status - The status of the Run, can be `Complete`, `Running`, `Aborted`, or `NeedsAttention`
* Date Started - The date the Run was started
* Username - The name of the User who owns the Run
* Instrument Name - The name of the instrument where the Run was performed

{% screenshot /images/mobile-ios/runs-list.png %}

##See details about your Run

If you click on a Run from the Runs list page, you will be taken to a detailed page about that Run.  This page includes:

* Status (Running, Complete, Aborted) - The Run Status
* Name of the Owner User - The name of the user that owns the Run
* Name of the Instrument - The name of the instrument where this Run was executed
* Run Summary Information - Some summary information about the Run, including **Number of Cycles** and **%>=Q30**
* Run Charts - SAV charts from the Run.  You can scroll through the charts by swiping left and right on the charts in the page

{% screenshot /images/mobile-ios/runs-details.png %}

##View a list of your Analyses

If you click on the **Analysis** icon ![Analysis List Icon](/images/mobile-ios/analysis-unselected.png) on the bottom of the app, you will see the Analyses list page.  You can click on any Analysis in the list in order to view more details about that particular Analysis. On this page, you will see the following:

* Name of the Analysis - The name of the analysis, generally includes App Name and a Date/Time stamp
* Status - Overall status of the analysis
* Status Summary - A message to accompany the status of the analysis 

{% screenshot /images/mobile-ios/analysis-list.png %}

##See details about your Analysis

If you click on an Analysis from the Analysis list, you will be taken to a detailed view of your analysis.

This page includes:

- Analysis Name - name of the analysis
- Status - Overall status of the analysis
- Project - The Project where the results from this analysis are stored
- Application - The name of the application that was run
- Total Size - The total size of the output data that was generated from the analysis
- Duration - The duration of the analysis
- Multi-Node Details - details about the different nodes used for the analysis, this will show up if the app is configured to use multiple nodes for analysis

{% screenshot /images/mobile-ios/analysis-details.png %}


## Notifications

The **Notifications** icon ![Notifications Icon](/images/mobile-ios/notifications-unselected.png) will take you to the Notifications list in the app.  This list will show you the latest notications related to your Runs and Analyses.   

{% screenshot /images/mobile-ios/notifications.png %}


##Supported Version(s)

The iOS BaseSpace App is supported on any device running iOS version 1.6 or newer.