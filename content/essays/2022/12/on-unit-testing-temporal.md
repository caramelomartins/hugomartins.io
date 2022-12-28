---
title: "On Unit Testing in Temporal"
date: 2022-12-28
draft: false
authors: Hugo Martins
categories: [temporal, testing]
summary: "I was surprised by the ease with which I was able to implement unit testing in Temporal. It was quite a surprise."
---

I've been using [Temporal](https://temporal.io/), "a developer-first, open source platform that ensures the successful execution of services and applications", for troubleshooting purposes for a while now. We use it [quite extensively at Datadog](https://www.youtube.com/watch?v=LxgkAoTSI8Q). Recently, I've started actually implementing something on top of Temporal and was amazed at the simplicity of its testing framework.

At first I was a bit confused, as I'm interested in Go and could only find documentation focusing on Java. Eventually, I found [How to test Workflow Definitions in Go](https://docs.temporal.io/go/how-to-test-workflow-definitions-in-go), a great introduction to the testing framework that Temporal offers with [testsuite](https://pkg.go.dev/go.temporal.io/sdk@v1.17.0/testsuite).

It all starts with a `WorkflowTestSuite`, in which we can initialize a `TestWorkflowEnvironment`. This `TestWorkflowEnvironment` is powerful and we immediately get access to a few important functions, such as `AssertExpectations` or `ExecuteWorkflow`. These allow you to do things as diverse as checking that all expected calls (to mocks) have been made and to execute a workflow inside a test environment. We can use `RegisterActivity` or `RegisterWorkflow` as well, to ensure our Activities and Workflows are available in the environment we'll execute our tests in.

Unfortunately, as pointed out in Temporal's documentation, *"unless the Activity invocations are mocked(...) the test environment will execute the actual Activity code including any calls to outside services."* This is particularly harmful if the Activities being invoked execute code that makes calls to endpoints on a network because: 

- it makes the tests unable to execute without connecting to the network; 
- it makes for unreliable and inconsistent behaviour of the tests; 
- and it doesn't allow us to test multiple failure modes.

For this reason, Temporal's testing framework offers methods to mock Activities in a `TestWorkflowEnvironment` such as `OnActivity`. This works well when the Activities are implemented internally but I couldn't get it to work when the Activities are implemented in a third-party package.

I read a bit of the [temporalio/samples-go](https://github.com/temporalio/samples-go/blob/main/expense/workflow_test.go) repository, which houses a collection of samples and accompanying tests, to understand what needs to be done. It is *seems* simple enough:

```go
type UnitTestSuite struct {
    suite.Suite
    testsuite.WorkflowTestSuite
}

func (s *UnitTestSuite) TestSampleWorkflow() {
	// Initialize test environment for UnitTestSuite.
	env = s.NewTestWorkflowEnvironment()

	// Register needed Activities.
	env.RegisterActivity(externalpackage.ExternalActivity)

	// Mock Activities.
	env.OnActivity(externalpackage.ExternalActivity, mock.Anything, mock.Anything).Return(nil)

	// Execute Workflow and Assert.
	env.ExecuteWorkflow(SampleWorkflow)
	(...)
}
```

I was surprised it was this easy. I confess I was expecting a little bit more complexity. That might be due to my lack of knowledge about the internals of Temporal, but I have to say I was quite excited about how easily I was able to make progress.
