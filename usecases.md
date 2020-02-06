Possible use cases for machine learning in QE processes
===============

# Categorizing issues

It would be useful to categorize issues along two dimensions: _seriousness_ and _difficulty_. The worst issues are blockers (highest seriousness) which are impossible to fix (highest seriousness) :).

The idea would be that at the time of creation, the person who found the issue (or is opening it) should rank it for seriousness (e.g. on a scale of 1 to 10). Similarly, at the time of closing an issue, the person who fixed it should rank it for difficulty. It would be ideal if the issue had further tags: component in question, type (network, docs, tests, UI, ...), etc. Over time, we'd have data which could be crunched to estimate the difficulty of the fix required for an opened issue (assuming the seriousness would be given by the person who opened the issue).

Knowing a difficulty estimate for an fix to an issue would help with planning and work assignments (e.g. give harder issues to seasoned engineers).

# PR checks

Currently, our PR checks involve running complete testsuite against each push to a PR. This is basically a regression test every time. It would be tempting to run only a deterministic subset of tests against a PR depending on which code is affected directly (e.g. a function `fun1` got changed that is called by `fun2` in the code. So all code that calls `fun2` with parameters that cause call of `fun1` should be tested too). However, this could be insufficient or imperfect.

It would seem beneficial to analyse the relationships between codebase and issues, creating a heatmap of causal links (this change in codebase caused this bug/issue). Consequently, if PR changes an existing component, we would trigger tests which touch everything within a certain 'distance' (in the heatmap/graph) from the change. It would be on top of the deterministic PR test checks. Figuring out the distance (or even a more complicated metric) would be the job of the ML tools. What are the probabilities that someting at distance 3 is affected? Is it worth testing? Recommendations to that effect would help.

When the testbase increases and it becomes infeasible/impractical to run all tests for each PR check, using AI to recommend/choose subset of tests on top of some core subset would save computational time and/or increase reliability of PR checks.

# Test design (integration testing)

It is not difficult to imagine an automated test design if a suitable (schematic) product map is supplied. In other way, draw a graph with components/features/logical units as vertices and arrows (directed edges) as allowed transitions between the features or components. Pass this as input to the ML engine and receive a set of user stories in test format. 

The ML/AI part of the solution would be in figuring out which user stories are more likely to reveal flaws. The key ingredient will be designing a good graph at first. Then the data collection will lead to heatmaps on that graph, and statistical analysis will then help choose the suitable set of walks on the graph (user stories). 


