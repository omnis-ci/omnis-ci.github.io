---
layout: post
title:  "OmnisTAP Diagnostics and Code Coverage"
date:   2018-03-19 10:49:36 -0400
categories: omnistap diagnostics coverage announcement
author: aclay
excerpt_separator: <!--end_excerpt-->
---

By popular demand, I've added a series of diagnostics to command-line runs of OmnisTAP. These diagnostics are detailed on the [OmnisTAP wiki](https://github.com/suransys/omnistap/wiki/Running-OmnisTAP-from-the-command-line#diagnostics). The most popular request has been to measure code coverage, which is part of these diagnostics.

<!--end_excerpt-->

Adding performance tracking induces a slight overhead to the test run. Per the Omnis documentation, collecting performance data has a slight impact:

> There is a very small overhead when data is collected...

There is a more noticeable impact when performance data is gathered at the end of the run and written to disk. OmnisTAP will output the time it takes to write these diagnostics:

```
Mon Mar 19 10:24:04 2018 Notice: Profiling stopped in 12 seconds
Mon Mar 19 10:24:04 2018 Notice: Test types logged in 0 seconds
Mon Mar 19 10:24:05 2018 Notice: Timings logged in 1 second
Mon Mar 19 10:27:28 2018 Notice: Profile logged in 3 minutes, 22 seconds
Mon Mar 19 10:27:29 2018 Notice: Coverage logged in 1 second
```

After running our builds through these changes, I took a look for potential "low-hanging fruit" to optimize OmnisTAP runs. This would help offset the performance hit for tracking coverage and speed up builds in general.

We use the mocker quite heavily in our tests: 4,682 times in the last test to be precise. The mocker creates a subclass of your application class when calling `$mock()`, then deletes it when the test completes. This helps the mocker follow live changes to your application code during development and keep your development library clean of test subclasses.

However, this class creation and deletion comes at a cost, which the profiler cleared showed. Calling `ogTAPSuper.$mock()` comprised **29%** of our test run, and calling `ogTAPManager.$_teardownMocks()` to remove the mock classes used 2.8% of the total run.

Our builds operate by copying in clean libraries to the test runtimes for each build, so I decided to change the mocker to not delete classes during a command-line run. This won't cause any problems with tests running properly and the extra mocking subclasses will be thrown away after a build. With this change in place, `ogTAPSuper.$mock()` dropped to about 11% of the run, and tearing down mocks dropped to 1.9%.

I don't anticipate this change will cause an issue if you preserve your libraries from test run to test run. You'll  build up a cache of testing subclasses over time, but I can't predicate how that might cause a problem. If it does, [post an issue](https://github.com/suransys/omnistap/issues/new) to the OmnisTAP github project with details and we'll get it sorted out.

Have fun boosting your coverage and optimizing your code!