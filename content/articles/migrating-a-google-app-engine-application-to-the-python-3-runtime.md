---
title: "Migrating a Google App Engine Application to the Python 3 Runtime"
date: 2022-07-25T00:00:00-06:00
draft: false
description: Reflections on migrating a large-scale Google App Engine application to Python 3.
tags: ["programming", "python", "google-app-engine"]
---

Starting [January 1, 2020](https://www.python.org/doc/sunset-python-2), the Python community stopped support for Python 2. As a consequence, platforms such as Google App Engine recommended that applications using the Python 2 runtime be updated to the Python 3 runtime as soon as possible.

This article details the steps our team took to ensure a successful migration of our Google App Engine applications from Python 2 to Python 3.

## Philosophy

>"Too many cooks spoil the broth."

The *Mythical Man-Month: Essays on Software Engineering* is a book on software engineering and project management by Fred Brooks. The prevailing theme of the text is his discussion of Brooks's law: *Adding manpower to a late software project makes it later*. We found the optimal number of Engineers working on the migration was **three**. Less and the undertaking would become overwhelming. More and the work of management and communication began to outweigh the work of the actual migration.

This small, but effective group of engineers was tasked with identifying and completing tasks. Having three engineers working on the project reduced amount of work in-flight, which ensured effective communication and reduced duplicative effort. The cycle of issue identification, remediation, and review was a tight loop and gave all members an understanding of changes being made.

## Communication

Using a combination of Slack, Jira, and GitHub, progress was broadcast across multiple channels. This redundancy was helpful, since it ensured everyone was on the same page. It became crucial to review these channels of communication several times throughout the day in order to stay up-to-date on the progress of the project, identify blockers, and provide help/feedback.

- [ ] Create a Slack channel.
- [ ] Create a Jira epic (include epic on all tickets).
- [ ] Create a Kanban board using epic.

## Code Management

### Code Reviews

Three proved to be a magic number in code reviews as well: one engineer submits a patch, two engineers review the change. All parties are kept aware of changes and the requirement of having two approvals on a pull request ensured code quality remained high.

### The integration branch

It is highly unlikely that the application will be backwards compatible with Python 2 after the migration. Such efforts will probably prove futile. Instead, create a branch that will comprise all changes required for the migration to Python 3 and never look back.

- [ ] Create an integration branch (ex. `python3`).

**NOTE**: We found making backwards compatible changes to the master branch to be a low return on investment approach to reducing the scope of the code change required for the migration.

### Keeping the integration branch current

Rebasing a branch causes the entire commit history to be rehashed. If the integration branch is rehashed, all branches that are derived from that branch will also need to be rehashed. This causes unnecessary work. If commits are added to master, these changes should be merged into the integration branch instead of rebasing the integration branch on master:

Given the following:

```
      A---B---C topic
     /
D---E---F---G master
```

Rebase:

```
              A'--B'--C' topic
             /
D---E---F---G master
```

Merge:

```
      A---B---C topic
     /         \
D---E---F---G---H master

```

## Testing

### Unit Testing

Our team relied heavily on the robustness of our unit tests in order to ensure proper functionality of our application after migrating from Python 2 to Python 3. Using a standard red/green approach, changes to application logic that produced a failing unit test where accompanied by a patch to the unit test.

**NOTE**: Be wary of [Mock](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock). If the attributes set on a Mock object do not reflect the attributes that the application receives at runtime, errors will evade detection.

**Example**

*Bad*
```python
mock_response = Mock(data="data")
```

*Good*
```python
mock_response = Mock(data=b"data")
```

The response object ([flask.Response](https://flask.palletsprojects.com/en/2.1.x/api/#flask.Response)) returns the encoded string representation of the response body (`bytes` object).

This is just one of many possible examples where an improper attribute did not capture the actual input for our application at runtime.

### Splitting Traffic

Ideally, a deployment should pass from a development environment to a production environment with a near certainty that it does not introduce any regressions. For most applications (including ours), this is not the case. We found that our most productive testing of the application came from allowing a small percentage of production traffic to reach the upgraded application. This was accomplished using Google App Engine's [traffic splitting](https://cloud.google.com/appengine/docs/standard/python3/splitting-traffic) functionality.

>You can use traffic splitting to specify a percentage distribution of traffic across two or more of the versions within a service. Splitting traffic allows you to conduct [A/B testing](https://en.wikipedia.org/wiki/A/B_testing) between your versions and provides control over the pace when rolling out features.

## Logging

Google App Engine application logging is sent to a dedicated Splunk indexer (`gae_logs`). Furthermore, log fields are parsed and can be used to construct useful queries:

```
index=gae_logs project_id=w-rmconsole versionId=python3
```

* `index=gae_logs`: Google App Engine index
* `project_id=w-rmconsole`: Google App Engine project ID
* `versionId=python3`: Google App Engine application version

**NOTE**: It may be helpful to increment the version number of the Google App Engine deployment (ex. `v1, v2, v3`) in order to more easily differentiate logs as errors are addressed.

Logging in the Python 3 runtime follows the logging standard in [Cloud Logging](https://cloud.google.com/logging). In the Python 3 runtime, app logs are no longer bundled with the request logs but are separated in different records. See the following guide for setting up Cloud Logging for Python:
* [Setting Up Cloud Logging for Python](https://cloud.google.com/logging/docs/setup/python)

## Error Monitoring

From the [Google documentation](https://cloud.google.com/error-reporting),

>Error Reporting counts, analyzes, and aggregates the crashes in your running cloud services.

Generally, the platform running an application is responsible for handling unrecoverable runtime errors in your application. As there does not seem to be a proper alternative for capturing these errors in App Engine, we found that leveraging the Error Reporting product to be highly effective.

Runtime errors can be easily viewed and associated with a Jira ticket for remediation.

## Technical Challenges

### Bytes vs. Strings

The following serve as a good primer before addressing errors related to byte strings and character strings in Python 3:
* [The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses)
* [Strings, Unicode, and Bytes in Python 3: Everything You Always Wanted to Know](https://betterprogramming.pub/strings-unicode-and-bytes-in-python-3-everything-you-always-wanted-to-know-27dc02ff2686)

In Python 2, the default type for strings was `str`, however its value was stores as `bytes`. Furthermore, Python 2 allowed for the implicit conversion of `bytes` objects to `str` objects and visa versa. In Python 3, this is no longer the case. `str` and `bytes` are now strictly separate types:

* `str` corresponds to the now deprecated `unicode` type in Python 2. It is represented internally as a sequence of Unicode codepoints.
* `bytes` roughly corresponds to the former `str` type in Python 2. It is an immutable sequences of single bytes.

**NOTE**: A `bytes` object has no inherent encoding, so you must know the encoding if you want to try and decode it!

**Best Practices**

Ned Batchhelder coined the term *unicode sandwich* to describe the practice of handling byte and character strings in Python. This practice has the following characteristic:

>"Bytes on the outside, unicode on the inside, encode/decode at the edges."

The idea is to use a `str` object when processing text, thereby getting access to the wide-range of methods for string processing. When you are dealing with input/output however (filesystems, external APIs, etc.), use `bytes`.

### Legacy bundled services

Previously, Google App Engine provided several scalable, proprietary services for app development. While these services are still available in the Python 3 runtime, Google recommends [migrating bundled App Engine services](https://cloud.google.com/appengine/docs/legacy/standard/python/migrate-to-python3/migrating-services).

### Specifying dependencies

In the Python 2 runtime, dependencies were installed alongside the application (ex. `pip install requirements.txt -t lib/`). However, dependencies for Python applications are declared in a standard `requirements.txt` file. For example:

```
Flask==0.10.1
google-cloud-storage
```

When you deploy to App Engine, the dependencies specified in the `requirements.txt` file will be installed automatically with your deployed app.

### Major Package upgrades

Our team was fortunate enough to only have to do one major rewrite of a module that consumed a library comprising major changes to its API ([google-cloud-storage](https://googleapis.dev/python/storage/latest/client.html)).

## Conclusion

This guide touches on the major considerations when migrating to the Python 3 runtime. It is, however, not exhaustive. For Google's full guide on migrating to the Python 3 runtime, see the following article:
* [Migrating to the Python 3 runtime](https://cloud.google.com/appengine/docs/legacy/standard/python/migrate-to-python3)
