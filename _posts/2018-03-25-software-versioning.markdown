---
layout: post
title:  "Software Versioning"
date:   2018-03-25 00:00:00 +0300
categories: software versioning
comments: true
---

### Introduction

This document will explain how we version software projects when we publish a new release [@kitapyurdu](https://github.com/kitapyurdu). Explained versioning logic is built on my experience that I gained from leading and developing software projects up-to-now. 

In recent years, we have heavily used [redmine](http://www.redmine.org/) as an issue tracker at our software development life cycle. Although its main concepts come from issue types of [redmine](http://www.redmine.org/), all those concepts are common and issue-tracker agnostic. Functionality, use case, user scenario are all equivalent of each other according to this document's perspective.

### Four Token (a.b.c.d) Versioning

We use **a.b.c.d** kind of versioning:

```
(major).(feature).(revision).(bug/refactoring)
```

`a` = Major version number, which denotes that release contains backward incompatible major changes. For most of the time, it contains rewriting software or changing all the architecture that current and previous software are very different from each other. It's very rare to change it. When it's updated new software version contains lots of new features and some important modifications.

`b` = Feature number, which is incremented by number of newly added features. 
  * What is a **feature**? 
    * If an issue is type of **feature**, it means that it's adding some new functionality to the software which doesn't exist before. Little or big, it brings something new into table.

`c` = Revision number, which is incremented by number of newly added revisions. 
  * What is a **revision**? 
    * If an issue is type of **revision**, it means that it's extending a functionality that exists before, especially to cover up some misunderstandings at development process or to make the functionality more user-friendly and beneficial. It does not change the main description and goal of that functionality. But it modifies, deletes or adds something to the flow in order to make it more accepted and adopted.
    
`d` = Bug or refactoring number, which is incremented by number of newly added bug fixes or refactoring issues. 
  * What is a **bug**?
    * If an issue is type of **bug**, it means that it fixes a software defect. So there is no change in functionality or external behaviour when you look from end-user's perspective, but there is an internal fix.
  * What is a **refactoring**?
    * If an issue is type of **refactoring**, it means that it doesn't change external behavior or fix a defect. It improves internal code quality or contains some QoS improvements. We can think a refactoring as an enhancement. Generally refactoring improves nonfunctional attributes of the software. For example; improvement of code readability and  maintainability, reduction of complexity, improved extensibility for future changes, more expressive internal architecture.
    
By comparing consecutive two version numbers you can identify number and types of issues done at a glance. Below there are some examples which show how to increment version number when you are ready to release your software:
   * 2.1.3.1 --> 1 feature --> 2.2.0.0
   * 2.1.3.1 --> 1 feature, 3 revisions --> 2.2.3.0
   * 2.1.3.1 --> 2 features, 1 revision, 4 bugs --> 2.3.1.3
   * 2.1.3.1 --> 2 bugs, 1 refactoring --> 2.1.3.4
   * 2.1.3.1 --> 1 feature, 5 bugs --> 2.2.0.5
   * 2.1.3.1 --> 2 revisions, 1 bug ---> 2.1.5.2
   * 2.1.3.1 --> 3 revisions --> 2.1.6.0
   
As you notice, when you increment a most significant part, the least significant parts after that part are reset to <<zero>>. For this reason, if you have a version number that does not indicate all done issues up-to-now. But if you have two version numbers one after another, or a new relase has just come; you know roughly done issues between the two releases. It's like a numeric summary of a changelog.

Notes:
  * most significant: nearest to **major** (to the left)
  * least significant: far from **major** (to the right)

### Conclusion

There are different schemes on software versioning. For example, [this](https://stackoverflow.com/questions/3377655/code-version-change-rules/) stackoverflow entry discusses some of them. On one aspect, it's a personal or corporate choice. Your issue types, release schedules, CI practices, software development lifecycle and sometimes the product or application itself defines your versioning scheme. 

[Semantic versioning](https://semver.org/) is also an interesting resource to take a look.

Happy coding.

{% include share-post-on-twitter.html %}