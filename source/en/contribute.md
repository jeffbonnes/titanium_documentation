<summary>
	There are many different ways to contribute to Titanium and its community, and your involvement is welcomed and appreciated. This document will explain:

* The numerous ways you can participate to help Titanium improve
* How to submit bug reports effectively
* How to obtain all of Appcelerator's code and documentation
* How to use Git and Github to submit code

</summary>

# What is Contributing?

There is typically a very narrow definition that springs to mind when people talk about "contributing" in relation to software projects. While obviously Titanium wouldn't exist without code, you don't need to be a black-belt in Java or Objective-C to make a worthwhile contribution to it.

Titanium has been brought to this point through the efforts of people with a very broad range of skills and experience, from those who are totally non-technical, to the newbie, to the web developer through to those who have already developed their own native applications, and many more besides. There are always things to be done that, however small, can collectively make Titanium evolve more rapidly.

The approach of someone wishing to contribute to Titanium will ideally be one that begins at the community. By engaging through exchanging questions and answers, proposing ideas, noticing bugs, publishing workarounds and similar activities, you get a much better idea of what Titanium is all about, including how it functions and how it can be improved. In addition to helping you to become a more proficient developer, your feedback is crucial to the core team as it helps set the direction of the project. This ensures Titanium will be as useful as possible to the most people.

This document describe the ways that you can get involved, some of which you may already be doing as a matter of routine. If you haven't considered them as *true* contribution before, then you may have overlooked their value. If these activities are done with the community in mind, and following a few simple guidelines, their results will be a tremendous benefit to everyone.

## How to Contribute to Titanium:

* participate in the Q&A
    * ask questions
    * vote - even voting bad information down, is a contribution!
	 * help others
* find and report bugs, using the outlines in the section below
    * watch bug tickets that are important to you
* fix bugs and submit patches
* write modules and share them with the community
* correct and translate documentation

These are explained in more detail in the following sections.

# Participating in the Q&amp;A
The Questions and Answers forum, or [Q&A](http://developer.appcelerator.com/questions/created) as it's known, has the potential of being an unbeatable place to get help, with every conceivable issue being recorded and resolved. We use it ourselves, to discover the main concerns troubling the community, and also to take part in the discussion. Crucially, its made possible by having such capable and generous members willing to give their time to assist others.

As it is the first place they usually go when they need assistance, the Q&A and its community is one of the things that makes Titanium accessible for new users. Without its support, the learning-curve can be too steep. Consequently, it plays a direct role in the community's growth.

You may think that the Q&A is primarily dependent on those who help. Indeed, these people are vital. However, it's the quality of information in the questions that drives it. For instance, unless a question is given an informative and relevant title, how will anyone find it in the future? More to the point, how will *you* find it in future?

When you have been using the Q&A for some time, you may notice a recurring trend, which is that some questions get answered almost immediately, while others are left with no responses at all.

So, what is the secret to getting answers?

## Q&amp;A - Jumping the queue

When people help out in the Q&A, it's often a spontaneous gesture. They are likely to be more drawn to questions that offer something to learn or that they can determine at a glance that they know the answer.

<info>Tip: Grab attention, by creating a question title that directly relates to your core issue, in the form of a question that can be easily answered.</info>

In spite of their unwavering good nature, Helpers in the Q&A like an easy life just as much as anybody else. They won't always have the time to read through long accounts of the frustrations you feel you have endured while attempting to get something to work. If they can't quickly understand what the issue or question is, they may simply move on to the next one. Hence:

<info>Tip: Stay on topic - explain your problem concisely, but in full.</info>

Your question is competing with many others for the attention of a Helper, so they aren't likely to wait around long enough to request information you've left out. Even if they do, do you really want the delay?

<info>Tip: Include all the relevant information in your original post. And, yes, mobile platform and Titanium SDK version are almost always relevant!</info>

If you include some code that demonstrates your issue, you not only increase your chances of getting an answer, but also of getting the correct answer more quickly.

<info>Tip: include a use-case; see [Creating Good Use-cases](#creating_good_use-cases) for guidelines. If there is a lot of code, use [gist.github](https://gist.github.com) or [pastebin](http://pastebin.com), but confirm that it never expires so that others can make sense of your question in future. Use an online zip sharing websites, such as [nippyzip](http://www.nippyzip.com), to include any assets required to make it work.</info>

If your problem produces an error or exception, there is no need to attempt to explain it. By including the verbatim message in your question, you give Helpers a much better insight into the issue.

<info>Tip: always include any error or exception messages by copying and pasting rather than explaining it. If it is long, use [gist.github](https://gist.github.com) or [pastebin](http://pastebin.com), but check first that they don't expire.</info>

<info>Tip: for emulator issues, including application install failures, include the emulator trace output.</info>

Did you know that people wishing to help can have questions delivered to them via email, by subscribing to specific tags? Hence, those expert in iphone-related matters may subscribe to the "iphone" tag and those who know about install issues may subscribe to the "install" tag. Therefore, if you want your question to reach the relevant people, you should set quality tags when you post it.

<info>Tip: always set some relevant question tags. At the very least, they should describe your mobile platform and Titanium SDK version.</info>

Using the "Mark as the best answer" option to close your question enables you to easily show your appreciation with bonus forum points for a Helper's time and effort. They will no doubt recall your good turn when they see your name against your next question. Also, doing so helps everyone to find reliable answers later on, as the questions with "best" answers are easy to spot in the search results.

Even if no answers provided an exact solution, don't be mean! Under the answer that inspired you most to figure it out for yourself, add a comment explaining the actual solution and then mark it as best.

<info>Tip: for each of your questions, add a comment to the closest answer, giving the actual solution, and then mark it as best. Consider using the "My Q&A" link to review your old questions, and see if any more can be closed in this way.</info>

Lastly, voting up or down helps improve the accuracy of the Q&A search tool, because it allows good answers to be prioritized. This benefits everyone, including you, when searching for solutions.

## The Question Checklist

Here's a convenient checklist that you may use for reference when creating new questions.

![question checklist](../assets/images/guides/contribute/question-and-answer-checklist-v1.png)

# Submitting Bugs

We use [Lighthouse](https://appcelerator.lighthouseapp.com/projects/32238-titanium-mobile/tickets?q=state%3Aopen) as our bug ticketing system. It has a [powerful search syntax](http://help.lighthouseapp.com/faqs/getting-started/how-do-i-search-for-tickets) that you should get into the habit of using when you see any unexpected behavior from Titanium. This will save you a lot of time debugging issues with the core code that are impossible for you to fix, when you could be looking for a workaround instead.

If you find a ticket that describes a bug that is important to your project, use the "watch ticket" facility to be kept up-to-date by email of any changes to its status. This is also useful because we can use the total number of watchers to help us prioritize our work.

Having the whole community's eyes on the code helps make Titanium more stable and featureful much more quickly. If you find a bug, and can prove *with certainty* that it exists, then we will be glad to hear about it.

However, creating tickets in Lighthouse should not be taken lightly, as incorrect submissions will create more work for our development team, taking up valuable coding time. Also, resist the temptation to start discussions within existing tickets, as this will also slow the development process.

## The Bug-Reporting Lifecycle

![bug reporting lifecycle](../assets/images/guides/contribute/bug-reporting-lifecycle-v1.png)

The steps are as follows:

1. find possible bug
2. search Lighthouse thoroughly for similar issues
3. create a use-case, following the guidance in [Creating Good Use-cases](#creating_good_use-cases)
4. search the [Q&A](http://developer.appcelerator.com/questions/created) for the issue and its solution. If none exists, post a question to garner community opinion and request a workaround, following the guidance in [Q&A - Jumping the queue](#q&a_-_jumping_the_queue). Include your use-case
5. once certain of a bug:
    * create a Lighthouse ticket (one ticket per bug)
    * keep it as concise and as factual as possible
    * include your use-case
    * watch ticket, to receive updates about its status
6. help others by posting the ticket's URL to your Q&A question as a comment under someone else's answer and mark it as "best"

## Creating Good Use-cases

A "use-case" or "use-case code" is a working script that demonstrates a specific bug. The *best* use-case is one that contains the least possible code while still demonstrating the bug.

Being in the habit of writing use-cases can be really helpful when you are troubleshooting an issue, because it can differentiate user coding errors from actual bugs.

For example, you observe that the `layout:'vertical'` property of Titanium's window object does not work as expected for the window's child views. In the process of producing a use-case, you remove every property from both the window and its sub-views except those that are absolutely necessary. With less code, it is often easier to notice a mistake and, in this case, you realize that the children's top property had been inadvertently set, which caused the views to be positioned absolutely rather than relatively.

Syntax errors can be the cause of unusual behavior from your code. Therefore, there is no point escalating an issue to the community forum or the bug ticketing system until you have ensured it validates successfully.   See our [Third-party tools](tool_thirdparty.html) page for recommendations of good validators, or use [JSLint](http://www.jslint.com) online.

In summary, to create a use-case:

* remove any superfluous windows, views and controls
* rely on default values where possible; remove any properties that are not absolutely necessary
* use a Javascript validator to ensure that your code is free of syntax errors
* ensure your code runs without any modification when copied into a new project

# Contributing Code and Documentation

## Finding the Code

All our open source projects are hosted on [GitHub](http://www.github.com/appcelerator).

You are welcome to download the code in a single archived file, using the "Downloads" button provided for each repository. For example, this is the button for the [titanium_mobile](https://github.com/appcelerator/titanium_mobile) repository:

![Github Downloads Button Screenshot](../assets/images/guides/contribute/github-downloads-button.png)

However, the recommended way is to use [Git](http://git-scm.com), the Version Control System (VCS). This makes it much easier for you to keep your local copy synchronized with the public version, while also letting you submit code and documentation changes to the core team, if you wish. For more information about how to use it, see the [Git and Github](#git_and_github) section below.

## Finding the License

The Appcelerator Titanium SDK and its associated documentation are [open source](http://en.wikipedia.org/wiki/Open-source_software) and licensed under the [Apache Public License (version 2)](http://www.apache.org/licenses/LICENSE-2.0.html).

## Before you can Contribute Code

Since Appcelerator itself is both an open source project and also a Company that is in charge of its long-term development, we require that you sign a standard Open Source Contributors License Agreement (CLA). As with most open source projects, legal issues regarding intellectual property rights require us to protect ourselves, our contributors and our users from any future ownership claims from third-parties.

Thus, you must first electronically sign the [CLA](http://bit.ly/app_cla) before you can contribute code, documentation or any other materials to the project.

## Compiling the Code

In order to build one of the Titanium software projects, for example `titanium_mobile`, you will need the following tools:

* Python 2.6+ 
* SCons
* Platform SDKs such as Android, iOS (if building on OSX), etc.

Once you have a local copy of the code, open a command line (shell) and run:

<code>
	cd /path/to/titanium_mobile
	scons
</code>

### The code structure

Of the compiled files, the following are of particular interest:

<table>
<tr>
	<th>File or Directory</th> <th>Purpose</th>
</tr>
<tr>
	<td>`/dist/mobilesdk-*.zip`</td>
	<td>Compiled Titanium SDK, that can be installed to Titanium Developer</td>
</tr>
<tr>
	<td>`/dist/android/javadoc/`</td>
	<td>Titanium Android Java API in HTML (note: not the Titanium javascript API!)</td>
</tr>
</table>

To contribute to the Android SDK, you will need a knowledge of Java. See the [Module Developer Guide](module_android.html) for Android for more information.

To contribute to the iOS SDK, you will need to know Objective-C. Refer to the [Module Developer Guide](module_ios.html) for iOS.

## Generating the Documentation

In order to generate one of Titanium's documentation projects, such as `titanium_documentation`, you will need the following tools:

* Python 2.6+ 
* mako
* markdown

Once you have a local copy of the files, from the command line (shell), run:

<code>
    cd /path/to/titanium_documentation
    ./gen.py
</code>

This will generate a complete website in the `output` directory.  You can then view the webpage `output/en/index.html` in a browser .

### The documentation structure

The source files for each guide is under a language folder (such as `en`) and the documentation is written in [Markdown](http://daringfireball.net/projects/markdown/) format.

The documentation structure is specified in the `doc.json` file in the root directory. The file is a JSON-formatted file that contains the information that the generator script uses to build the entire website.

Each documentation page uses a template under the `templates` folder. The template is in a format that uses the [Mako](http://www.makotemplates.org/docs/syntax.html) templating system for Python. Generally, you will use the existing `guide.html` template already provided.

### Localization of the guides

To localize for a specific language, you must add the language to the `doc.json` file.

For each `language` entry in the `languages` array, the generator will attempt to build a localized version of the documentation:

<code>
"languages":
[
	{
		"directory":"en",
		"title":"English",
		"code":"en"
	}
]
</code>

To add a new language, add a new entry under the `languages` key. The value for the `title` key should be in the specific language.  For example, for Japanese the key should be: 日本. The code should be the ISO language code.

# Git and Github

## Getting Started with Git

Git can be a little daunting if you've never used it before, but most people soon learn to love it. The following resources will help you to get started:

* [Git Official Website](http://git-scm.com)
* Michael Marner's [Git Tutorial](http://www.youtube.com/user/MichaelMarner#p/u/9/HaSDIdNkCDQ) (video)
* Github [Introduction to Git](http://learn.github.com/p/intro.html) (video)
* Linus Torvalds' [Git Tech Talk](http://www.youtube.com/watch?v=4XpnKHJAok8) (video)
* The [Pro Git Book](http://progit.org/book/)
* the `git help` command and the [Git online manual](http://www.kernel.org/pub/software/scm/git/docs/)
* Github [Git Reference](http://gitref.org) (manual)
* the Freenode IRC server's #git channel
* and [many others](http://www.google.com/search?hl=en&q=git+version+control)

## Git Prerequisites

Before you start, you will need to work through these points:

* configure SSH, required for pushing branches to Github.  See "Generating SSH Keys" for [Linux](http://help.github.com/linux-key-setup/), [Mac OSX](http://help.github.com/mac-key-setup/) and [Windows](http://help.github.com/msysgit-key-setup/).

## Some Quick Git Tips:

The following can make Git a little easier to use:

* use [gitk](http://gitk.sourceforge.net/development.html) to give a visual representation of your repository
* by default, if a file's permissions change, Git will perceive it just the same as if its contents were changed, Thus, to ignore permissions changes, run  `git config core.filemode false`


## Committing Code with Git in 7 Easy Stages

The following diagram describes the full development cycle used to contribute code or documentation to an Appcelerator repository, using Git and Github.

![bug reporting lifecycle diagram](../assets/images/guides/contribute/contribute-workflow-v3.png)
Download a hi-res version [here](../assets/images/guides/contribute/contribute-workflow-v3-1680.png)

### Committing code with Git in 7 easy stages (detailed)

<note>In the examples in this section, a Github account with the name "appcel" is being used for demonstration purposes.</note>

### 1. Fork the public repository
Click on one of [Appcelerator's Public Repositories](https://github.com/appcelerator) on Github, such as [titanium_mobile](https://github.com/appcelerator/titanium_mobile), and press the "fork" button near the top right-hand corner of the page:

![fork titanium project on github screenshot](../assets/images/guides/contribute/github-fork-repo.png)

Once the process is complete, go to your Github Dashboard and click on your new public repository, `&lt;your-account&gt;/titanium_mobile`, that has been forked from the Appcelerator repository.

![fork titanium project on github screenshot](../assets/images/guides/contribute/github-fork-dashboard.png)

<note>Although your repository is identical to the original at the moment that the fork occurs, from that point onwards each of them exists totally independently of the other. Thus, your repository will not receive any commits made to the original, unless you explicitly, manually push them to it. Having said this, you will leave the existing branches in their current state, and instead use this repository to publish your new, up-to-date, development branches.</note>

### 2. Clone your repository, and configure git remotes

In Github, click on the **SSH** button, to return a secure URL, and copy the URL to your computer's clipboard:

![github ssh url screenshot](../assets/images/guides/contribute/github-ssh-url.png)

<info>Using the SSH protocol with Git allows your system to authenticate with the remote repository. This is only necessary if you intend to push to it.</info>

At your local machine's command line, change to the directory where you wish to store the repository, and use the SSH URL with `git clone`, as follows:

<code>
git clone git@github.com:<your-account>/titanium_mobile.git
</code>

Now return to Appcelerator's public repository, [titanium_mobile](https://github.com/appcelerator/titanium_mobile), and click the **Git Read-Only** button. This will show a git URL.

![github ssh url screenshot](../assets/images/guides/contribute/github-ssh-url-appcelerator.png)

<info>As the Git protocol is the most efficient for transferring data over a network, the Git URL is preferred when you do not need to authenticate with a repository. As you won't ever push code directly to Appcelerator's repositories, you don't need an SSH URL to access them.</info>

Add the URL as a "remote" named `upstream`:
<code>
git remote add upstream git://github.com/appcelerator/titanium_mobile.git
</code>

Then inspect your remotes:
<code>
git remote -v
</code>

Which should show you:
<pre>
origin	   git@github.com:&lt;your-account&gt;/titanium_mobile.git
upstream   git://github.com/appcelerator/titanium_mobile.git
</pre>

### 3. Create a git development branch locally, based on the master branch
In order to fix a bug in the `master` branch, you must first checkout that branch as follows:

<code>
git checkout master
</code>

Now create a branch based on master:
<code>
git checkout -b ticketNumber-description
</code>

You have now created a branch named `ticketNumber-description` (eg for Lighthouse ticket [#1331](https://appcelerator.lighthouseapp.com/projects/32238-titanium-mobile/tickets/1331) you could create branch `1331-viewMissingChildrenProperty`)

<info>For each bug that you tackle, there should be an existing Lighthouse ticket describing the problem. Thus, ideally, a bugfix should contain the fix for a single ticket only.</info>

### 4. Develop your Titanium code, and rebase
Now develop your code as you normally would, committing regularly to protect your work.

Then bring your local branch up-to-date with changes in the public repository:

* commit or [stash](http://ariejan.net/2008/04/23/git-using-the-stash/) all of your work
* checkout master
* pull changes from upstream master into your local master
* checkout your development branch
* [rebase](http://progit.org/book/ch3-6.html) your development branch with the master branch, and resolve any conflicts

In other words, run the following code:
<code>
git commit // [follow the usual procedure of git add etc]
git checkout master
git pull upstream master
git checkout ticketNumber-description
git rebase master
</code>

<info>You can update your local branch at any time with the latest commits to Appcelerator's public repository using this method. However, it's most important to do so just before you push your finished work, to reduce the risk of conflicts when it is merged.</info>

<warning>
When you rebase a branch, it can change the SHA-1 hash of any of your commits since it branched off from master. If these commits have been made public and someone has subsequently based their work on them, rebasing will cause problems because these old commits will appear to be new commits and will have to be merged despite already existing.

As no-one will be using your branch to work from, this should not be an issue. However, the easiest rule to follow is **never rebase a branch that has already been made public/pushed**.

Hence, if your branch has been pushed, skip the `git rebase master` step.
</warning>

### 5. Squash your commits

To leave a cleaner history of commits when your branch is pulled into Appcelerator's public repository, squash all your commits into a single commit:

* git reset --soft HEAD~noOfCommitsToSquash
* git commit

For example, to squash the last 3 commits into one:

<code>
git reset --soft HEAD~3
git commit
</code>

<warning>
Like rebase, this also rewrites the history, so the caveat mentioned above also applies. Hence, **skip this step if the branch has already been pushed**.
</warning>

### 6. Push your local branch to your Github repository

To make it available to others, push your completed local development branch to your own public Github repository. This will create a new branch with the same name on your origin remote:

* git push origin ticketNumber-description

For example:

<code>
git push origin 1331-viewMissingChildrenProperty
</code>

### 7. Send a pull request to the Appcelerator team

To raise a Github pull request, follow these steps:

* at your Github `titanium_mobile` repository, select the development branch that you have pushed using the branch selector on the left
* confirm that your last commit is displayed beneath
* click the "Pull Request" button

![screenshot of your github repository](../assets/images/guides/contribute/github-pull-request.png)

Then, add a message using the form on the following page, and click the "Send Pull Request" button to submit it.

<info>
A pull request is a request for the HEAD of the branch. Any commits that you push to your branch, up until the moment the merge is actioned, will be merged into upstream/master.
</info>

# Our Thanks

Last, but by no means least, thank you for being interested-enough in Titanium to have read this far and for any subsequent contributions you make.

