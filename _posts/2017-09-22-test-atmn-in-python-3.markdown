---
layout: post
title:  "Test Automation in Python III - Pytest"
date:   2017-09-22 23:40:25
categories: generic
tags: 
image: /assets/article_images/2017-09-22-test-atmn-in-python-3/time.jpg
image2: /assets/article_images/2017-09-22-test-atmn-in-python-3/time_mobile.jpg
---

Python has several test frameworks to choose from. The one I would like to bring to your attention is pytest, which is a wonderfully simple yet very powerful test framework.

Let's dive straight into a demonstration of a pytest test in its most basic form.

To begin to use pytest, you would first need to download and install the pytest package into your virtualenv using pip:

{% highlight shell_terminal %}
(venv) $ pip install pytest
{% endhighlight %}

This should install the latest version of python (at the time of writing the version is 3.2.2).

In a new project folder, create a new file named "test_file_example.py" and enter the following contents:

{% highlight python %}
def test_function_example():
    assert 2 == 1 + 1
{% endhighlight %}

In this example, we are simply asserting that the value of 2 is equal to the sum of 1 + 1. The test file is named "test_file_example" and the test function is named "test_function_example". Already I hope to have demonstrated to you that the amount of code required to construct simple test is minimal. The equivalent in the JUnit5 framework for Java would be:

{% highlight java %}
import static org.junit.jupiter.api.Assertions.assertEquals;

import org.junit.jupiter.api.Test;

class FirstJUnit5Tests {

    @Test
    void myFirstTest() {
        assertEquals(2, 1 + 1);
    }

}
{% endhighlight %}

If we have our virtualenv with pytest installed as a package (can be confirmed by listing your installed packages with with **pip freeze**), then we can simply run the test by executing the following in terminal within your new project directory:

{% highlight shell_terminal %}
(venv) $ py.test
{% endhighlight %}

And this will produce a output like so:

{% highlight shell_terminal %}
================================= test session starts =================================
platform darwin -- Python 3.6.0, pytest-3.2.2, py-1.4.34, pluggy-0.4.0
rootdir: /Users/kylemcevoy/blogexample, inifile:
collected 1 item

test_file_example.py .

============================== 1 passed in 0.01 seconds ===============================
{% endhighlight %}

So this is just a very simple test being run. We have shown that with python and pytest we are not limited to just an object-oriented design structure and that we do not need to write any boilerplate code or inherit any test related base classes for our tests. The pytest test runner has auto-discovery of tests and identifies tests through the prefix "test_" first by the filenames and then by function names (filenames can alternatively be suffixed with "_test", though this doesn't apply to function names).

Let's change the contents of the test file to the following:

{% highlight python %}
def number_three():
    return 3

def test_function_example():
    assert 2 == number_three()
{% endhighlight %}

So we have a function *number_three*, which returns an integer of 3, that is under test and we have a test function which is attempting to assert that method call *number_three()* returns 2. Running the test again returns the following:

{% highlight shell_terminal %}
================================= test session starts =================================
platform darwin -- Python 3.6.0, pytest-3.2.2, py-1.4.34, pluggy-0.4.0
rootdir: /Users/kylemcevoy/blogexample, inifile:
collected 1 item

test_file_example.py F

====================================== FAILURES =======================================
________________________________ test_function_example ________________________________

    def test_function_example():
>       assert 2 == number_three()
E       assert 2 == 3
E        +  where 3 = number_three()

test_file_example.py:5: AssertionError
============================== 1 failed in 0.06 seconds ===============================
{% endhighlight %}

This is an example of an assertion failure and the detail level of the output that pytest provides. More examples of the level of detail that pytest provides can be found [here](https://docs.pytest.org/en/latest/example/reportingdemo.html#tbreportdemo).

Test auto-discovery, negligible boilerplate and detailed failure introspection are great features however my favourite feature of pytest is its implementation and facilitation of **test fixtures**. 

Test fixtures are predominantly used in the form of carrying out necessary set up as a prerequisite in order to execute a test but can also involve some post process following a test such as clean up.

Under object-oriented test structures, such as JUnit, test classes facilitate test fixtures through setUp and tearDown methods (e.g. methods are annotated with *@BeforeAll*, *@BeforeEach*, *@AfterEach*, *@AfterAll*). Whilst pytest still facilitates this, as there are many advantages to using this OO approach, pytest provides the ability to apply test fixtures to both independent test functions and test functions within test classes by means of **dependency injection**. 

Diving straight into an example again, if we edit our previous test file *test_file_example.py* with the following contents:

{% highlight python %}
import pytest

@pytest.fixture
def three_fixture():
    return 3

def number_three():
    return 3

def test_function_example(three_fixture):
    assert three_fixture == number_three()
{% endhighlight %}

Here we have imported the pytest package at line 1 in order to be able to decorate the function *three_fixture* with *@pytest.fixture*. We have our function *number_three*, which is our function under test, as well as our test function *test_function_example*. In this case we have entered *three_fixture* as an argument to our test function which I wanted to draw your attention to but I will explain later what exactly is happening.

If we run this test, you will see the test will pass. This means the assertion statement has resolved to asserting that 3 does in fact equal 3.

By decorating the function with *@pytest.fixture* and importing this function name as an argument of a test, we are injecting the return of that function into our test. So to explain what pytest is doing, at a high-level in ordered steps, when running the py.test command:

1. Test auto-discovery kicks in and identifies the test file *test_file_example.py* and within that test function *test_function_example*
2. Pytest then assesses that test function's defined arguments, finding a fixture named *three_fixture* and executes that fixture function *before* it enters the test logic.
3. The executed fixture returns the integer 3.
4. The assertion statement has reference to a variable *three_fixture* which contains the returned value from the point above.
5. The assertion statement attempts to equate the variable *three_fixture* (of value 3) with the method call *number_three()* which returns 3 at the point of being called within the test.

Now this demonstration isn't showing a setup in the true sense of the word (returning the integer 3 isn't setting up anything), I am just showing how a function can be switched into a fixture, by use of the pytest decorator, and how that fixture can be imported (or rather *injected*) into a test function and then accessed via a variable. As the pytest documentation puts it, "fixture functions take the role of the injector and test functions are the consumers of fixture objects."

In pytest, each fixture needs to have a unique name and, when not co-located within the same file as a test function, need to be declared at module, class or, my personal preference, at project level. For the purposes of this post, I will provide more details on the declaration of fixtures, for use within a project, in a future post as this is closely tied to a design pattern of test fixtures that I discuss in a later post in the series.

Teardowns in pytest can take two forms; you can house a teardown in the same fixture as a setup or house a teardown in its own independent fixture. Here is an example of a setup and teardown in the same fixture (note that this is an illustration and will not run within your *test_file_example.py*):


{% highlight python %}
import pytest

@pytest.fixture
def api_client(api):
    api.login()
    yield api
    api.logout()
{% endhighlight %}

The above test fixture:

1. Imports an *api* fixture, which is ran before the *api_client* fixture's logic begins to execute. (Yes, that's right a fixture within a fixture).
2. The *api_client* fixture calls the *login* method on a returned hypothetical api object instance as a setup.
3. The *api_client* fixture returns the instance to the consumer of the fixture
4. Once the consumer has exited their function, the *logout* is subsequently execute as a means of teardown.

Again, whilst this may not be a realistic example it is demonstrating the ability to house test setup and tear down within the same test fixture.

Having a modular implemention of test fixtures allows for a much simpler and scalable way to use, reuse and share test setups and teardowns within and amongst projects.

There are many more features as well as some handy nuances, in pytest, that bring about a very powerful and flexible test framework which is excellently placed to deal with larger test automation projects. I hope this post have given you a brief insight into its key features, if you would like to find out more please do check out their [website](https://docs.pytest.org/en/latest/). In the next posts, I will show you how to write functional tests for apis and browsers using pytest and other tools.

I would finally like to take the opportunity to thank all the contributors of pytest for creating an excellent framework which is not only immensely useful but an absolute pleasure to work with.