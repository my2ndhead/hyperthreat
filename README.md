# Splunk Hyperthreat App Suite
*INSIDER THREAT DETECTION FOR THE MASSES*

# Introduction

The Splunk Hyperthreat app suite provides advanced Insider Threat Detection. The App Suite consists of several independent and separately usable Splunk apps/add-ons

- Risk Manager: The main app for detecting and reporting risk events and scoring risk objects
- Hyperbaseline: A general purpose baselining add-on to monitor activity and detect outliers
- Hypercrypto: An add-on that provides hash and crypto commands.

# Why using the Hyperthreat App Suite?

Detecting insiders is a tough task, as insiders can move slowly over several weeks and months to stay undetected. On the other hand, opening security incidents for every little suspicious action, will cause many false alerts.

A good solution to find insiders is to define risk scores for suspicious events. Each risk event is related to a risk object. A risk object could e.g. be a user/employee doing something bad, or a system/host being attacked. If a risk object behaves badly over time, the risk score will rise and possibly reach a level that requires further investigations.

Hyperthreat brings a complete and free solution that covers risk scoring baselining and encryption to the masses.

# Why did we build it?

After doing tens of security projects, an often asked question was, how to properly do insider threat detection. Almost every week, there are news about data breaches. Monitoring every little suspicious action, will cause too many false positives. The risk here is, that over time security incident will be just "clicked away". On the other hand, privacy concerns often prevent the implementation of a monitoring system that tracks individual employees.

After talking with many customers and gathering all requirements, we came up with the Hyperthreat App Suite.

# How does it work?

For detecting risk events, a normal Splunk alert search is created. When a risk event occurs, the search fires and calls an alert script provided by the Risk Manager app (risk_handler.py). This will cause Risk Manager to take care of the alert.

Inside the Risk Manager App, a user with the Splunk Risk Manager Role defines which alerts are managed by Risk Manager. For each alert, a risk object type, such as user or host, is defined, that will be monitored. For each risk object of this type, a risk score will be assigned to the risk object. Risk scores are accumulated, should a risk object be again detected by an alert.

If needed (e.g. to collect evidence) search results can automatically be stored inside a KV store collection.

# How can an anomalous behavior be detected for a risk object?

To detect, if a risk object is behaving differently than usual, a baseline has to be set up. Out-of-the-box Splunk does not provide baselining functionality. This is the reason why we brought Hyperbaseline into the App Suite.

# Is it allowed to monitor and analyze employees?

In most countries, privacy laws are very restrictive and are getting tighter all the time. Care has to be taken, about who is allowed to see critical events. Collecting data about employee activity is usually ok, as long as access is restricted to the log data. Analyzing employee activity in detail has very tight restrictions. This is why we developed Hypercrypto, because what's the use of all these features, when you're not allowed to use them.

The Hypercrypto add-on brings hashing and encryption algorithms to Splunk. 

Using the "hash" command, a user's name can be hashed. The hash value will always be identical for the same user and can therefore be used for scoring. A secret salt will make it very hard to reveal the original username.

Using the "crypt" command, all evidence that needs to be collected can be encrypted. We decided to implement a asymmetric encryption algorithms (public-key encryption), so that any users with the appropriate rights is able to encrypt data without the need to share passwords. Reading the date will need access to the private key and optionally, but recommended, a password. Passwords to private keys are stored inside Splunk and access to the key
password is done one per user basis. This way a HR or a legal department may be the only one who can decrypt the data.

# Doesn't the Splunk App for Enterprise Security already provide risk scoring and insider threat intelligence?

Yes, but besides being a premium app and out of reach for many Splunk user, several key features are missing in ES:

- Collecting and storing evidence data
- Easy general purpose baselining commands
- Securely monitoring insider threats with hashing and encryption algorithms, making the Hyperthreat App Suite the only insider threat detection solution that respects employee privacy.

Beside this, the Hyperthreat App Suite can always be used together with the ES App.

# Has the app been tested in production?

Not yet, as it is hot of the press. But our first tests against the DARPA insider threat test data, has shown 100% success in detecting an insider in the sample data (R6.1.1). Further tests are planned and algorithms are improved as we learn more.

# Can I integrate solution xyz into the Hyperthreat App Suite?

Yes! Risk scoring is done using normal Splunk searches. You can of course integrate your existing anomaly detection solution, security device etc. all the data that you already have in Splunk! You can also encrypt this data if needed.

# Where can I find more details about how the apps work?

Complete documentation can be found here

- Risk Manager: https://github.com/my2ndhead/risk_manager/wiki
- Hyperbaseline: https://github.com/my2ndhead/SA-hyperbaseline/blob/master/README.md
- Hypercrypto: https://github.com/my2ndhead/SA-hypercrypto/blob/master/README.md

# Will you provide support for the App Suite?

- Currenty there is no commercial support, but depending on the success of the app we might consider this
- Bugs and enhancement requests can reported to us by opening an issue on Github.
- Splunk App Certification is a thing we are considering

# Roadmap

- Adding support to encrypt and hash data during index-time
- Better integration of alert searches with Risk Manager, with a future Splunk feature that may or may not exist
- More reports, dashboards
- Seamless integration with Alert Manager for Incident Management
- UI improvments for Hypercrypto
- UI improvments for Hyperbaseline
- We will listen to our users and add features our users are needing

# License
- **This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.** [1]
- **Commercial Use, Excerpt from CC BY-NC-SA 4.0:**
  - "A commercial use is one primarily intended for commercial advantage or monetary compensation."
- **In case of the Hyperthreat this translates to:**
  - You may use the Hyperthreat Suite in commercial environments for handling in-house Splunk alerts
  - You may use Hyperthreat Suite as part of your consulting or integration work, if you're considered to be working on behalf of your customer. The customer will be the licensee of Hyperthreat Suite and must comply according to the license terms
  - You are not allowed to sell the Hyperthreat Suite as a standalone product or within an application bundle
  - If you want to use Hyperthreat Suite outside of these license terms, please contact us and we will find a solution

# Notes for the Splunk Apptitude2 Challenge

- We have spun up an Amazon EC2 cloud instance and will provide full access to the operating system (Ubuntu) and Splunk Enterprise.
- The Splunk instance contains the DARPA test data, and TA-threatintelligence.
- Also, the GA release of the Hyperthreat Suite, including Risk Manager, Hyperbaseline and Hypercrypto will be installed as documented.
- A separate App with Demo searches will be provided. As the test data is historic and due to lack of time it was impossible to write an event re-player, all the searches are run against historical data. All searches simulate the situation as how they would be in real-life. https://github.com/my2ndhead/SA-hyperthreat-demo/blob/master/README.md
- The tests are run against the R6.1 Test data and focus on the Insider #1 with the username of "CSF2712".
- Risk Manager contains minor parts of code from the Alert Manager-app, where all intellectual property is owned by us (code reuse).

# What we are proud of...

This was a real team effort. We were able to build a complex solution together, touching almost every Splunk technology there is. Everyone was able to show his strengths. The culmination of the project was, when we brought the three different app pieces together, almost everything worked right out-of-the-box and the insider in the test data was found immediately.

## References
[1] http://creativecommons.org/licenses/by-nc-sa/4.0/
