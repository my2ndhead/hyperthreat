# Splunk Hyperthreat App Suite
*Insider Threat Detection for the Masses*

# Introduction

The Splunk Hyperthreat app suite provides advanced Insider Threat Detection. The Suite consists of several Splunk apps/add-ons

- Risk Manager: The main app for detecting and reporting risk events and scoring risk objects
- Hyperbaseline: A baselining add-on to monitor activity and detect outliers
- Hypercrypto: An add-on that provides hash and crypto commands.

# Why should you use the Hyperthreat App Suite?

Detecting insiders is a tough task, as insiders can move slowly over several weeks and months to stay undetected. On the other hand, opening security incidents for every little suspicious action, will cause many false alerts.

A good solution to find insiders is to define risk scores to suspicious events. Each risk event is related to a risk object. A risk object could e.g. be a user/employee doing something bad, or a system/host being attacked. If a risk object behaves badly over time, the risk score will rise and possibly reach a level that requires further actions.

# How does it work?

For detecting risk events, a normal Splunk alert search is created. The search should fire, when a risk event happens. The alert is set up to call an alert script provided by the Risk Manager app (risk_handler.py). This will cause Risk Manager to take care of the alert.

Inside the Risk Manager App, a user with the Splunk Risk Manager Role defines which alerts are managed by Risk Manager. For each alert, a risk object type, such as user or host, is defined, that will be monitored. For each risk object of this type, a risk score will be assigned to the risk object. Risk scores are accumulated, should a risk object be again detected by an alert.

If needed (e.g. to collect evidence) search results can automatically be stored inside a KV store collection.

# How can an anomalous behavior be detected for a risk object?

To detect, if a risk object is behaving differently than usual, a baseline has to be set up. Out-of-the-box Splunk does not provide baselining functionality. This is the reason why we brought Hyperbaseline into the Suite....

# Isn't employee monitoring forbidden in most countries?

In most countries, privacy laws are very restrictive. Care has to be taken, about who is allowed to see critical events. Collecting data about employee activity is usually ok, as long as access is restricted to the log files. Analyzing employee activity in detail has very tight restrictions. This is why we developed Hypercrypto.

The Hypercrypt add-on brings hashing and encrpytion algorithms to Splunk. 

Using the "hash" command, a user's name can be hashed. The hash value will always be identical for the same user and can therefore be used for scoring. Using a secret salt, will make it very hard to reveal the original username.

Using the "encrypt" command, all evidence that needs to be collected can be encrypted. We decided to implement a asymmetric encryption algorithms (public-key encryption), so that only users who need to see the data, will need the password for the private key. Passwords to private keys are stored inside Splunk and access to private key passwords is done one per user basis. This way, HR or a legal department can decide, wether further investigation have to proceed.

# Doens't the Splunk App for Enterprise Security provides risk scoring, insider-threat intelligence?

Yes, but besides being a premium app and out of reach for many Splunk user, several key features are missing in ES:

- Collecting and storing evidence
- Easy baselining commands
- Securely monitoring insider threats with hashing and encryption algorithms, making the Hyperthreat app the only app that respects employee privacy.

# Has the app been tested in production?

Not yet, as it is hot of the press. But our first tests against the DARPA insider threat testdata, has shown 100% success in detecting an insider in the sample data (R6.1.1). Further tests are planned and algorithms are improved as we learn more.

# Where can I find more details how the Apps work?

Complete documentation can be found here

- Risk Manager: https://github.com/my2ndhead/risk_manager/wiki
- Hyperbaseline: https://github.com/my2ndhead/SA-hyperbaseline/blob/master/README.md
- Hypercrypto: https://github.com/my2ndhead/SA-hypercrypto/blob/master/README.md

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

- We have spun up an Amazon EC2 cloud instance and will provide full access to the operating system (Ubunt) and Splunk Enterprise.
- The Splunk instance contains the DARPA test data, and TA-threatintelligence.
- Also, the GA release of the Hyperthreat Suite, including Risk Manager, Hyperbaseline and Hypercrypto will be installed as documented.
- A separate App with Demo searches will be provided. As the testdata is historic and due to lack of time it was impossible to write an event replayer, all the searches are run against historical data. All searches simulate the situation as how they would be in real-life. 
- The tests are run against the R6.1 Test data and focus on the Insider #1 with the username of "CSF2712".
- Risk Manager contains minor parts of code from the Alert Manager-app, where all intellectual property is owned by us (code reuse).

## References
[1] http://creativecommons.org/licenses/by-nc-sa/4.0/
