# FestPass Jenkins Challenge

## Submission Details

- **Git Author / Alias:** `anubhutianurag`
- **Jenkins Job:** `festpass-ci`
- **Git Repository:** `git://127.0.0.1:9418/festpass-java`
- **Branch:** `main`
- **Jenkinsfile:** `Jenkinsfile`
- **Build Agent:** `student-laptop`
- **Class Code:** `001`
- **Challenge Code:** `00842`

## Project Overview

FestPass is a Java-based campus event registration application that allows users to manage events and student bookings through a command-line interface.

The project includes functionality to add events, list available events, book tickets, cancel bookings, view event rosters, and generate booking summaries.

## Jenkins CI/CD Pipeline

The project uses Jenkins for continuous integration and automated testing.

The pipeline is configured to:
1. Check out the source code from the Git repository.
2. Compile the Java application.
3. Execute automated tests.
4. Build the executable JAR after successful testing.
5. Run smoke tests against the JAR.
6. Save the build artifact and submission evidence.

The pipeline also uses SCM polling to detect repository changes automatically.

## Commit History

| Commit | Message | Description |
|---|---|---|
| `36b5a42` | Configure my Jenkins assignment | Added the Jenkinsfile polling trigger and submission properties. |
| `2d290a4` | Add FestPass project files | Added the project files required by the pipeline. |
| `2c37a9d` | Add submission codes | Updated the class and challenge codes. |
| `e084af0` | Personalize my help introduction | Personalized the application help message. |
| `155d39d` | Fix cancel so it removes the booking | Fixed the booking cancellation functionality. |

*Note: Verify these commit IDs against `git log` before submitting the evidence.*

## Bug Fix: Booking Cancellation

### Problem

The original `BookingService.cancel` method checked whether a student had a booking but did not remove the student from the event's booking list.

This caused three test failures:

- `cancellationFreesSeat`: Cancelled seats were not available for new bookings.
- `cancellationsSurviveReload`: Cancellation was not persisted correctly.
- `cliWorkflowPersistsAcrossCommands`: The CLI continued displaying cancelled bookings.

### Solution

The following statement was added to remove the student from the event's booking list after validating the cancellation:

```java
event.students.remove(studentId);
```

This ensures that cancelled bookings are removed from the collection used by the application.

As a result, seats can become available again, and the application's booking list reflects the cancellation.

## Build Results

### Red Build — Build #5

- **Trigger:** SCM change
- **Commit:** `e084af0`
- **Result:** FAILURE
- **Tests:** 28
- **Failures:** 3
- **Errors:** 0

The three failures were related to booking cancellation. The JAR-building, smoke-testing, and artifact-saving stages were skipped.

### Green Build — Build #6

- **Trigger:** SCM change
- **Commit:** `155d39d`
- **Result:** SUCCESS
- **Tests:** 28
- **Failures:** 0
- **Errors:** 0

All automated tests passed after the cancellation fix.

## Application Usage

Run the application using Java:

```bash
java -jar build/festpass.jar help
```

### Available Commands

```text
add-event ID "TITLE" CAPACITY
list
book EVENT_ID STUDENT_ID
cancel EVENT_ID STUDENT_ID
roster EVENT_ID
summary
help
```

The help output identifies the application as personalized for `anubhutianurag`.

## Evidence Files

The submission evidence is maintained alongside the build artifacts.

- **Red-build evidence:** `evidence/red-build-5/submission-evidence.csv`
- **Green-build evidence:** `evidence/green-build-6/submission-evidence.csv`
- **Green-build artifact:** `evidence/green-build-6/festpass.jar`

The recorded SHA-256 hash of the original green-build JAR is:

```text
8e9348b67867bf5d941533200898fb90d59a4ec95c1f371b9a1645c3837a71b2
```

If the JAR is rebuilt or modified, regenerate the corresponding evidence and verify its SHA-256 hash.

## Conclusion

The FestPass Jenkins challenge demonstrates automated Java testing, SCM-triggered builds, artifact generation, and evidence collection. The cancellation bug was fixed by removing the cancelled student's booking from the event's student collection.

The successful build completed all 28 tests without failures or errors
