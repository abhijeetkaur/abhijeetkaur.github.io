---
layout: post
title:  GSoC 2017 with Zulip
tags:
- GSoC
- Google Summer of Code
- Zulip
---

## **Introduction**

This post is about my GSoC project, that I worked on during summer, 2017. I worked under the Zulip organization to improve and enhance Zulip bot system.

[Zulip](https://zulip.org/) is a powerful open source group chat application written in Python and uses the Django framework. My first contribution to Zulip made me realize the immense scope of improvement in the bots system.  Even through out the application period I focused my attention on refining and enhancing bots. Since I was already familiar with the bots system, I started contributing in the community bonding period itself.

I would like to express my heartfelt gratitude to my mentors [Tim Abbott](http://web.mit.edu/tabbott/www/), [Steve Howell](https://www.linkedin.com/in/stevehowell/) and the entire &quot;bots and integration&quot; team for making my summer such an incredible experience filled with a lot of brainstorming, learning and fun.

Communicating and working with people from around the globe was a learning experience in itself, as working on the same piece of code requires teamwork and collaboration.

With the end of this program I have learnt a lot about how open source works,  how to work with large codebases and collaborate with people from all over the world.

I had an amazing learning experience. I plan to continue to contribute to Zulip and actively participate in the weekly meetings as well.

## **Work done**

- **Test suite framework**
  - The summer started with making the existing bots structure (then contrib\_bots directory) rock solid, so that all the existing bots are consistent and working. This framework would also allow us to easily test new bots written by developers.
Initially one or two bots had unit tests for them, but these were independent. This required the need for creating a test suite framework for the existing bots and a common test library for all the bots.
  -  Initially, I did some [work](https://github.com/zulip/zulip/pull/4846). However, further discussion with the mentors led to some changes in the plan of action. The main challenge here was to come up with a structural framework that would be simple and clear.
  -  [Mock library](https://docs.python.org/3/library/unittest.mock.html) was used to mock BotHandlerApi class (which is the main library class for bots). Major focus was given in isolating all the testing to the bots folder and being independent of the rest of the code (by not using Client class).
  -  This also encouraged folks at [PyCon 2017](https://us.pycon.org/2017/community/sprints/) (which was taking place at the time I was working on this) to actively contribute more.
<br />(The final work done can be viewed [here](https://github.com/zulip/zulip/pull/4859))

- **Incoming webhook bot**
  -  My team-mate had completed work on adding incoming webhook bot and I worked on completing the frontend for creating an incoming webhook bot along with backend tests for the same.
  -  Working on this enhanced my understanding of incoming webhooks and bots workflow in general. I also got an insight into how the backend bots codebase interacts with the UI.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5082))

- **Writing bot tests**
  -  As the above mentioned test suite framework for bots got merged, test files for each bot had to be added. Initially, while working on the framework, I had only added test file for one bot as an example of the new structural framework.
  -  I added new test files for bots which are in accordance with the test-suite framework developed for contrib\_bots. Adding these tests made me run mostly all the bots in zulip&#39;s bot directory, further increasing my grasp and understanding of all the bots.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5094))

- **Mock internet service**
  -  On exploring further, we realized that another function (which was now being called indirectly) had to be mocked as some bots were directly calling it.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5101))
  -  By now, we had a good and stable testing framework. Now, we faced a challenge that the bots using the internet (like the giphy bot) would not always reply with the same response. This required us to focus on how we could add mock support for bots using internet services. I mocked http get requests calls. This now allows testing of bots (using third-party services) to be mocked and tested offline.
  -  Mocking the internet services led our tests to run faster.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5153))

- **Origin of Embedded vs External classes**
  -  Till now bots were a hidden part of the zulip community as they could only be run on local servers (by running run.py command in local development environment). The aim was to bring bots forward and be able to run on zulip servers. The bots running on the organization servers are known as Embedded bots.
  -  As we aimed to build on embedded bots, we did not want to restrict to a single  bot handler class. The common bot handler (BotHandlerApi) class was split into 2 bot handler classes: Embedded (EmbeddedBotHandler) and External bots (ExternalBotHandler). The former can be run on Zulip servers and the latter can be run on local servers. This way, both the classes have more scope of going into the specifications along with leveraging the same common functions.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5394))

- **View Bot type**
  - With Zulip now providing 3 different types of bots: Outgoing-webhook, incoming-webhook and generic bot, it was essential to display the bot-type in bot details.
  - The bots are listed under active/inactive tab, I added &#39;bot type&#39; to be displayed as one of the fields in bot details.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5354))
  - The bots are also listed under organization bots tab, I added &#39;bot type&#39; column to bots list in organization-settings.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5448))

- **Embedded bots set-up (in [zulip/zulip](https://github.com/zulip/zulip/))**
This involves the work done to get kick-started with Embedded bots. There was some initial backend work done on embedded bots but the workflow was incomplete and broken due to the highly dynamic nature of bots and integrations.
  - **Add Embedded bots logic to zerver**: The code added to split the common bot handler into two: External and Embedded, led PyPi package to break. To fix this we decided to move EmbeddedBotHandler class to a new file in `zerver/lib`, this also made sense so as to consolidate all code to run on the server at one place.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5504))
  - **Clean code:** Since, embedded bot handler and external bot handler classes use a few common functions, I removed redundant functions with minor adjustments, so as to keep the code from breaking.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5567))
  - **Fix bugs to run embedded bots**: Splitting the bots handler into two classes had the code flowing to and fro unnecessarily. I removed unnecessary and redundant calls to functions for efficiency.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5842))

- **Embedded bots set-up (in [zulip/python-zulip-api](https://github.com/zulip/python-zulip-api))**
  - This marks the time when, after a lot of brainstorming, the Zulip community decided to create a separate repository for its python API bindings (zulip/python-zulip-api). We shifted it, mainly because it was intuitive to not have it in the server. Moreover, interested folks can now directly explore and play around with our API bindings.
  - The separation of API bindings for zulip to a new repository, python-zulip-api, led to breaking of code at places. Complete end to end flow had to be checked in both the repositories for Embedded bots. I fixed code gaps.
<br />(The final work completed can be viewed [here](https://github.com/zulip/python-zulip-api/pull/17))

- **Add test fixture files**:
  - Regarding the tests for the bots relying on third party services, we were simply sending the request content and mocking the reply content directly. But now, we aim to send the entire http request and mock responses in a structured json file with headers. This enhances our testing capabilities by adding checks to a deeper level.
  - I worked on changing the entire bot structure to have test-fixture (even the offline bots). However, on further discussion, this plan was abandoned. Only the bots using internet were to have fixture files.
<br />(The work done can be viewed [here](https://github.com/zulip/python-zulip-api/pull/19))
  - Next, I worked on adding test-fixture for one of the bots, with complete coverage of corner cases. This required going through the exact format of request sent by the bot and the response coming from the internet.
<br />(The final work completed can be viewed [here](https://github.com/zulip/python-zulip-api/pull/21))
  - Later, I added test-fixtures for more bots. <br />(The final work completed can be viewed [here](https://github.com/zulip/python-zulip-api/pull/23))

- **Debug, clean and add test coverage**:
  - [Modify docs to call a bot correctly](https://github.com/zulip/python-zulip-api/pull/20).
  - [Add complete test-coverage for bot\_lib.py file](https://github.com/zulip/zulip/pull/5925).
  - [Debug google search bot](https://github.com/zulip/python-zulip-api/pull/29).
  - [Add complete test coverage for yoda bot](https://github.com/zulip/python-zulip-api/pull/54).
  - [Minor corrections in bots to make them run efficiently and correctly, while handling corner cases.](https://github.com/zulip/python-zulip-api/pull/60)
  - [Debug yoda bot](https://github.com/zulip/python-zulip-api/pull/68).
  - [Update docs to use bots](https://github.com/zulip/zulip/pull/4888).
  - [Debug codeflow breakage](https://github.com/zulip/python-zulip-api/pull/34).

- **Add authentication to embedded bots**
  - Now, we wanted a mechanism by which we could add authenticated bots as embedded bots. Not all bots are fit/eligible or desired to be able to run on the organization servers.
  - By discussion we formulated the need to add a registry where we would add embedded bots. Only these bots would be rendered in the UI, for user to choose from.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5882))

- **Embedded bots frontend**
  - Till now, for testing embedded bots, we had to manually use manage.py script to interact with the database directly. Now, it was time to add support for adding Embedded bots from UI.
  - This involved creating related Service object automatically (if the bot type is &quot;Embedded bot&quot;), and also displaying a list of authenticated embedded bots, as dropdown list, from our authenticated list of embedded bots.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/5811))

- **Consolidate Embedded-External bot system**
  - The Embedded bots system still had some shortcomings, such as not removing the initial at-mention call to the bots. Embedded bots needed to make use of a few utility functions from External bots library class.
  - This required bringing a few nested functions to the top and some modifications so that both the bot systems could leverage these.
<br />(The final work completed can be viewed [here](https://github.com/zulip/python-zulip-api/pull/64) and [here](https://github.com/zulip/python-zulip-api/pull/81))

- **Add bot-integration  widget**
  - This involved creating a widget that would directly create a bot from integration pages. Incoming webhook bots require the user to enter a url, that includes bot API key and the stream name (to which the bot is subscribed).
  - To do this, we first need to go to the organization settings page, create a bot, open the .zuliprc file and then copy the url. We also need to subscribe this bot to the stream of our choice. Then, we&#39;ll have to put these in the link provided.
  - All this can be avoided by using this widget, that intends to do all in one place.<br />
(The work done can be viewed [here](https://github.com/zulip/zulip/pull/5418))
  - Further worked on adding node tests for bot integration widget.<br />
(The work done can be viewed [here](https://github.com/zulip/zulip/pull/5700))

- **Add configuration handler**
  - There are bots, such as weather and yoda bots, that require API keys to access external APIs. For these bots, we read such specific bot-related configurations from the .conf file. For Embedded bots, we intend to take this configuration information input from the user and store it in the database.
  - Add config handler for bots, this involves adding support for embedded bots to store configuration settings in database.
<br />(The final work completed can be viewed [here](https://github.com/zulip/zulip/pull/6131))

- **Add state handler for bots**
  - This focuses mainly on the bots such as tictactoe, virtual\_fs, chess etc that need to store the state of interaction between the user and the bot.
  - Add state handler for bots, this enables bots to store the previous and current states of bots for future reference.<br />
(The work done can be viewed [here](https://github.com/zulip/zulip/pull/6182))

- **Manual testing command**
  - Since the bot testing was a bit cumbersome and time taking, we wanted to make the process of writing and testing both new as well as old bots really simple. We came up with an idea to create a command for it.
  - This eliminates the need to setup development environment, to create bots, setting up zuliprc file, subscribing the bot to the stream and then finally running the bot in order to try it out.
  - Manual command &#39;zulip-bot-output&#39; gives the bots response content directly.<br />
(The work done can be viewed [here](https://github.com/zulip/python-zulip-api/pull/90))

- **Other work**
<br />This involves minor (but significant) work that I did along the way.
  - **Add a new bot tab:**Creating a new tab that contains &quot;Add a new bot&quot; form, this was done because having multiple active/inactive bots made it cumbersome to find this form by scrolling below.<br />
(The work done can be viewed [here](https://github.com/zulip/zulip/pull/5096))
  - **Change tab on creating bot**: Creating a new bot (by filling out the bots related fields and clicking &quot;Create bot&quot; button) changes tab from &quot;Add a new bot&quot; to &quot;Active bots&quot;. This is done to make users know/confirm that the bot has been created and the user can view it in this tab.<br />
(The work done can be viewed [here](https://github.com/zulip/zulip/pull/5733))
  - **Display empty messages**: Add a line of text stating that there are no active or inactive bots. This is for better understanding of the user, as blank screen that
used to appear in case of no bots being present might seem broken
to some.<br />
(The work done can be viewed [here](https://github.com/zulip/zulip/pull/5788))

## **Commits**

All my merged work can be viewed here:

- In [zulip repository](https://github.com/zulip/zulip/pulls?q=is%3Apr+author%3Aabhijeetkaur+is%3Aclosed).
- In [python-zulip-api repository](https://github.com/zulip/python-zulip-api/pulls?q=is%3Apr+author%3Aabhijeetkaur+is%3Aclosed).

## **Future work**

- I have initialized the process for handling states in bots, further work needs to be done as to how the bots will make use of these utility functions. Since, no bot uses this newly added state-handler, I think writing down a bot making use of this will help us to complete this process.
- User-Interface for adding configurations for Embedded bots need to be added.
- Bot-integration widget&#39;s backend code is complete (along with tests), we need to discuss and figure out the user interface part for the same.