# Local Unit Testing Workshop
13th of December, year of our lord 2017
 
This workshop is focused on developing unit tests for our clean architecture enterprise projects.

## Objective: 
Learn and practice how to create local unit tests for warningapp-kotlin testing the following components:

* Interactors
* Presenters

## Program

**Introduction to the workshop covering the following topics:**

*View actions and lifecycle enabled MVP (Johnny)*

*Interfaces interfaces interfaces (Stinus)* -
Why all external dependencies used by anything other than the views should be wrapped in interfaces.

*Interactors and the pluggable executor (Stinus)*

**Quick explanation of the samples, how to use junit and how you mock dependencies with mockito.**

**Each team check out the warningapp-kotlin-android project and make a team branch.** 

**Each team gets two primary unit test assignments and optional secondary objectives if they have additional time.**

**Each team solve their assignments using the existing samples and documentation as a reference (and asking their colleagues ofc :)**


## Links
[warningapp-kotlin-android](https://github.com/nodes-projects/warningapp-kotlin-android)

[Mockito documentation](http://static.javadoc.io/org.mockito/mockito-core/2.12.0/org/mockito/Mockito.html)

[Vogella Mockito tutorial](http://www.vogella.com/tutorials/Mockito/article.html)


## Samples
The project contains 4 finished unit tests to serve as references/samples:

Presenters:

- [OnboardingPresenter](https://github.com/nodes-projects/warningapp-kotlin-android/blob/master/app/src/test/java/dk/nodes/marsling/presenters/OnboardingPresenterTest.java)
- [SelectWarningTypesPresenter](https://github.com/nodes-projects/warningapp-kotlin-android/blob/master/app/src/test/java/dk/nodes/marsling/presenters/SelectWarningTypesPresenterTest.java)

Interactors:

- [GetWarningsInteractorTest](https://github.com/nodes-projects/warningapp-kotlin-android/blob/master/app/src/test/java/dk/nodes/marsling/interactors/GetWarningsInteractorTest.java)
- [SetSubscriptionsInteractorTest](https://github.com/nodes-projects/warningapp-kotlin-android/blob/master/app/src/test/java/dk/nodes/marsling/interactors/SetSubscriptionsInteractorTest.java)

## Teams
Teams are organized as one team per office to faciliate cooperation. Because
of the difference in team sizes and because we actually want to use the spoils of your hard labor, the assignments are given per team. Common for all of them are:

Please commit them to your team branch (and not master or develop), please use the same package structure as well as naming scheme as in the examples.

Also you're supposed to test implementations, not the interfaces :). 

### Team CPH
Brian, Christian, Thomas, Johnny, Stinus

#### Assignments:
Write the local unit tests for the following components:

- SettingsPresenter 
- GetSettingsInteractor

#### Optional assignments:
- WarningsPresenter 
- SetSelectedWarningInteractor
- UpdateMyLocationInteractor
- SetMyLocationEnabledInteractor
- DownloadWarningInteractor
- PlaySoundInteractor
- GetWarningSoundsInteractor
- GetMyWarningsInteractor


### Team ARH
Morten, Mohammed

#### Assignments:

- SplashPresenter 
- SetOnboardingStatusInteractor

#### Optional assignments:

- QueryOnboardingStatusInteractor
- SelectSoundPresenter 
- SetSelectedWarningSoundInteractor

### Team LON
Vladimir, Filipé

#### Assignments:

- WarningPresenter
- GetSelectedWarningInteractor

#### Optional assignments:

- SubscribePresenter
- GetSubscriptionsInteractor
- GetWarningTypesInteractor
- SetSelectedWarningTypesInteractor




