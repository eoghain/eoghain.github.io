---
  layout: post
  title: Mocking objects in Swift
---

I was faced with a testing issue recently that made me think about testing and mocking objects in Swift in a different way.  Here I'll describe the thought process I went down and where I ended up.  Note this solution is only one way to do mocking and there are plenty of people working in this space creating full on frameworks like [Cuckoo](https://github.com/Brightify/Cuckoo) that you should investigate.  I unfortunately, was adding yet one more test to a project that is rife with Obj-C and Swift code and tests, and trying to find some simple way to ease some of my pain with mocking in Swift.

I'm sure we've all built code like this, you have a feature/action/screen that you only want to display to a user a single time (i.e. On-boarding).  So what do you do, you reach for your trusty `UserDefaults` and have at it.

```swift
class Onboarding {
  func show() {
    guard UserDefaults.standard.bool(forKey: "onboardingShown") == false else { return }
    UserDefaults.standard.set(true, forKey: "onboardingShown")
  
    // Show onboarding flow
  }
}
```

While that code works and will only allow your users to see the on-boarding flow a single time, how can you test it?  After a single test you need to delete the app from the device and reinstall to see it again, or remember to cleanup your UserDefaults storage after your tests but who does that?  So you get the bright idea that you should inject the `UserDefaults` object to use.

```swift
class Onboarding {
  var defaults: UserDefaults = UserDefaults.standard

  func show() {
    guard defaults.bool(forKey: "onboardingShown") == false else { return }
    defaults.set(true, forKey: "onboardingShown")
  
    // Show onboarding flow
  }
}
```

So far so good, you can now inject a different `UserDefaults` when testing so let's get to mocking.

```swift
class MockDefaults: UserDefaults {
  override func bool(forKey defaultName: String) -> Bool {
    return false
  }
  
  override func set(_ value: Bool, forKey defaultName: String) {
    // do nothing
  }
}
```

Great your mock object is now ready to return false to any bool request and absorb any setBool request without affecting the actual storage.  Problem solved test it, ship it, cash the paycheck.

But what happens when you have to test your guard, your mock always returns false.  You can't do that with this mock.  How could we fix this?

```swift
class MockDefaults: UserDefaults {
  var boolReturnValue: Bool = false
  
  override func bool(forKey defaultName: String) -> Bool {
    return boolReturnValue
  }
  
  override func set(_ value: Bool, forKey defaultName: String) {
    // do nothing
  }
}
```

Here we add a property to our Mock that we can set in each test and thus solve our problem.  This is good, and it solves the problem but explodes into many `ReturnValue` fields as the number of methods increases.

```swift
class MockDefaults: UserDefaults {
  var returnValues: [String:Any] = [:]
  
  override func bool(forKey defaultName: String) -> Bool {
    guard let returnValue = returnValues[defaultName] as? Bool else {
      return false
    }
    return returnValue
  }
  
  override func set(_ value: Bool, forKey defaultName: String) {
    // do nothing
  }
}
```

This solution solves the explosion of `ReturnValue` properties by containing them all in a nice dictionary.  However, this solution doesn't allow for any logic to be performed it just blindly returns whatever value you've given it just like the solution above, of course logic could be added into the methods in both of these solutions but it'd be one set of logic run for every test.  Also neither of these solutions solve for the case where you need to know that a specific method was called, otherwise known as `verify` in most mocking frameworks.  Of course you could add another property to show that a method was called or not and check that, but it wouldn't let you know if the method was called multiple times.

```swift
class MockDefaults: UserDefaults {
  var getBoolHandler: ((String) -> Bool)?
  
  override func bool(forKey defaultName: String) -> Bool {
    guard let handler = getBoolHandler else {
      fatalError("Missing handler for \(#function)")
    }
    return handler(defaultName)
  }
  
  var setBoolHandler: ((Bool, String) -> Void)?
  
  override func set(_ value: Bool, forKey defaultName: String) {
    guard let handler = setBoolHandler else {
      fatalError("Missing handler for \(#function)")
    }
    return handler(defaultName)
  }
}
```

Finally, we've come to the solution that made me write all of this down.  Here I'm using the same basic pattern from the above 2 solutions but instead of setting just a value I'm setting a block/closure/callback/handler to be called whenever the code being tested calls my Mocks methods.  This solves the problem of returning different values per test, solves the problem of not being able to do any logic in the mock, and allows me to solve the `verify` problem in my tests themselves.

```swift
func testShowOnboarding() {
  let mockDefaults: MockDefaults = MockDefaults()
  mockDefaults.getBoolHandler = { (name: String) in 
    return false
  }
  
  var flagSet: Bool = false
  mockDefaults.setBoolHandler = { (value: Bool, name: String) in 
    guard flagSet == false else {
      XCTFail("setBoolHandler called multiple times should only have been called once!")
    }
    
    flagSet = true
  }
  
  let onboarding: Onboarding = Onboarding()
  onboarding.defaults = mockDefaults
  onboarding.show()
  
  XCTAssertTrue(flagSet, "Didn't get past show() guard statement!")
}

func testShowOnboarding_Shown() {
  let mockDefaults: MockDefaults = MockDefaults()
  mockDefaults.getBoolHandler = { (name: String) in 
    return true
  }
  
  var flagSet: Bool = false
  mockDefaults.setBoolHandler = { (value: Bool, name: String) in 
    guard flagSet == false else {
      XCTFail("setBoolHandler called multiple times shouldn't even be called once!")
    }
    flagSet = true
  }
  
  let onboarding: Onboarding = Onboarding()
  onboarding.defaults = mockDefaults
  onboarding.show()
  
  XCTAssertFalse(flagSet, "Got past show() guard statement!")
}
```

I know this example is a bit contrived, but what blog post examples aren't?  Here I have 2 tests one to verify that I can get past my guard statement and one that verifies I can't.  I'm in complete control over what happens whenever one of my methods is called.  I've set multiple tests to verify that my code is doing exactly what I expect it to do and nothing more.  Looking at just my tests in the future I can easily understand what I'm testing for and if they started failing I should be able to understand easily why they failed. 

As I said at the beginning this isn't the end all be all of Mocking in unit testing.  It's just a journey I went through and thought it was interesting enough to write up and share (not that anyone reads my 3, now 4 blog posts 😀).