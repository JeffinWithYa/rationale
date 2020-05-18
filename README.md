# ![COVID Shield logo](/assets/covid-shield-logo.svg)

Our goal is to help Canadians to stay as safe as possible while the economy recovers and people return to work. We have done this by creating an app that uses exposure notifications (also known as ‘digital contact tracing’) to notify Canadians of potential exposure to COVID-19.

Combined with the [Apple & Google Exposure Notification API](https://www.apple.com/covid19/contacttracing), this is the most privacy-preserving course of action to help Canadians get safely back to work.

* [Overview](#overview)
* [Principles](#principles)
* [Privacy](#privacy)
* [Key decisions](#key-decisions)
* [Questions](#questions)
* [Additional resources](#additional-resources)

## Overview

We have built a minimal experience- and privacy-focused implementation of the exposure notification technology provided by Apple & Google. Our approach consists of three components: a mobile app, a backend server, and a web-based results portal. These three components work together to help notify users if they have potentially come in contact with a user who has received a positive COVID-19 test result.

![Three iOS devices showing the default screen, an exposure notification, and the detail screen in COVID Shield mobile app](https://github.com/CovidShield/rationale/raw/master/assets/ios-screens.png)

#### Mobile app

The (soon-to-be-open-sourced) mobile app collects and shares anonymized device identifiers ("random IDs") using Bluetooth Low Energy (BLE) as described in the [Apple/Google Exposure Notification documents](https://www.apple.com/covid19/contacttracing). The mobile app can be distributed and managed federally, provincially, or territorially and re-styled to match experience guidelines.

Whether distributed and managed at the federal, provincial, or territorial level, COVID Shield is able to connect with the centralized backend server. This app is unobtrusive in users’ lives by running in the background and only notifying users if they have possbily been exposed to COVID-19.

#### [Backend server](https://github.com/CovidShield/backend)
The backend server securely collects and stores diagnosis keys uploaded from the mobile app after confirmation of a positive test result. It also generates unique temporary codes which grant permission for people with a positive COVID-19 test result to upload their diagnosis keys.

#### Web portal (optional)
The (soon-to-be-open-sourced) web-based results portal is accessible only by health care professionals and can be deployed federally, provincially, or territorially. It provides unique temporary codes to health care professionals who then give those codes to users of the mobile app who have received a positive test result. This code gives the user access to upload their anonymized device identifiers (random IDs) via the app. There is no association between these temporary codes and specific tests or people. The code is delivered over any communications channel (such as the phone) so it cannot be traced to any person or their test results. These temporary codes are generated by the backend which also provides an API which makes the use of this portal optional.


### Diagrams

#### Normal usage

![A diagram outlining the normal usage workflow for COVID Shield](/assets/summary-diagram.png)

#### Positive test result

![A diagram outlining the positive test result workflow for COVID Shield](/assets/keyupload-diagram.png)

## Principles

Alongside the  [principles laid out by the Federal, Provincial and Territorial Privacy Commissioners](https://priv.gc.ca/en/opc-news/speeches/2020/s-d_20200507/), we use these principles to guide our work:

* Focus on individual consent, trust, and clarity
* Provide as much value as possible with a focused feature set
* Be simple enough for all Canadians to understand and use
* Avoid false positives by optimizing for test result certainty over self-reporting
* Be as unobtrusive as possible in people’s lives by requiring minimal interaction
* Request only the permissions necessary for exposure notification to work
* Give users only as much information as they need to make good decisions
* Delete all data 21 days after capture and decommission the app as soon as it is no longer useful
* Deliver openly and transparently so all interested users can explore the code

## Privacy

**COVID Shield has been designed with user privacy as the top priority.** We must strike a balance between opening the economy and preserving personal privacy, and COVID Shield takes a privacy-first approach. The COVID Shield app, its backend server, and the portal are all designed to make it as difficult as possible to personally identify users or link them to their test results. To learn more about how we’ve approached privacy, read [our privacy policy](/privacy-policy.md).

## Key decisions

#### Use Apple/Google Exposure Notification technology
The need for digital contact tracing is both urgent and important. While there are many possible approaches, we chose Apple and Google’s [Privacy-Preserving Contact Tracing](https://www.apple.com/covid19/contacttracing) for COVID Shield because it does not rely on any personally identifiable data.

#### Trust in test delivery
By becoming part of the test results delivery process, and not the test distribution or administration processes, COVID Shield can be used to connect more directly with patients with positive test results without massive integration efforts between test manufacturers, health agencies, and other organizations. The goal is maximum certainty with minimal required integration with existing health systems and software.

#### Federal database and federal, provincial, or territorial apps/portals
A centralized, shared database allows diagnosis keys to be distributed to all provinces and territories across Canada. Meanwhile, per-province/territory deployment and management of the mobile apps and web portals allows each region to operate in accordance with its own guidelines while still taking advantage of the national, shared database.

The ability to share diagnosis keys is especially important for Canadians living and working close to provincial and territorial borders as in the Ottawa/Gatineau region, or for those who travel between provinces.

#### Minimal exposure information
We believe it is important for the app to only provide enough information to make good decisions. Apple and Google provide  [more information about exposures](https://developer.apple.com/documentation/exposurenotification/enexposureinfo) such as number of exposures, duration of exposure, date of exposure, signal strength, and more. COVID Shield only shows users that they have been exposed within the past 14 days and does not provide the rest of this information.

We believe that providing this additional information could enable users to identify who exposed them and violate our privacy considerations. Users may also choose to interpret this data as a reason to not isolate or quarantine as needed and engage in riskier behaviour if they believe their exposure was insufficient to warrant isolation or quarantine. By limiting the information we are giving users, we are providing them only what they need to make good decisions about the risk to themselves or others, in conjunction with health care professionals.

#### Minimally intrusive
We chose to make COVID Shield minimally intrusive in users’ lives by running effectively in the background and only notifying users if they have been exposed to COVID-19. In an typical scenario, users run this app in the background and don't have to think about it again. If they have possibly been exposed, the app provides clear links to federal, provincial, or territorial guidance to help them make the most informed decisions.

#### Random IDs are stored securely on devices
Phones with the app installed use Bluetooth to collect and share random IDs with nearby phones. These IDs are stored securely on each phone. A user with a positive test result must receive a COVID Shield code from a health care professional to be granted access to share their random IDs, and the user must then grant permission to share their random IDs. Sharing is voluntary.

#### COVID Shield does not use location data such as GPS or WiFi signals
We do not request any device permissions outside of those necessary for Apple & Google’s Exposure Notification technology. This ensures that we are not accidentally collecting any data that we do not need for the explicit purpose of exposure notification.

#### Only people with confirmed positive test results can share their data
It will only be possible for a user to share random IDs if a positive COVID-19 test result has been reported by a health care professional. The health care professional will grant access via a numeric code. It will not be possible for bad actors to falsely report a positive test result.

#### Use of COVID Shield is voluntary
The user must choose to download and turn on the app, and they can choose to turn off or uninstall the app at any time. The random IDs sent by the app change every 10-20 minutes to prevent tracking. COVID Shield does not tie COVID-19 test results directly to app users, so their random IDs cannot be used to determine their identity. After receiving a positive test result, a user can choose to share random IDs. Sharing is voluntary and not required to continue receiving exposure notifications.

#### COVID Shield needs to be easy for all Canadians to use and understand
We designed COVID Shield for all Canadians. This means [writing in plain language](https://design.gccollab.ca/content/content-guidelines), ensuring the web portal meets [WCAG guidelines](https://www.w3.org/TR/WCAG20/), and focusing on clear information hierarchy and straightforward design.

#### COVID Shield is not a health app
We direct users to already published resources from Canadian health authorities. COVID Shield does not provide medical advice. It is complementary to any existing public health efforts, including traditional contact tracing performed by trained health care professionals.

## Questions

#### Who built COVID Shield?
We are a group of Shopify volunteers who want to help to slow the spread of COVID-19 by offering our skills and experience developing scalable, easy to use applications. We are releasing COVID Shield free of charge and with a flexible open-source license.

For questions, we can be reached at press@covidshield.app.

#### How can I contribute?

See [our guidelines](/CONTRIBUTING.md) for more information on contributing to the project.

## Additional resources

- [Office of the Privacy Commissioner of Canada joint statement on Privacy principles for contact tracing and similar apps](https://priv.gc.ca/en/opc-news/speeches/2020/s-d_20200507/)
- [Apple documentation on privacy-preserving contact tracing and exposure notifications](https://www.apple.com/covid19/contacttracing)
- [Apple & Google Exposure Notification FAQ](https://blog.google/documents/73/Exposure_Notification_-_FAQ_v1.1.pdf)
- [How privacy-first contact tracing works](https://ncase.me/contact-tracing/)