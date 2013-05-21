// Item 30

// Example method written under ARC.
if ([self shouldLogMessage]) {
    NSString *message = [[NSString alloc] initWithFormat:@"I am object, %p", self];
    NSLog(@"message = %@", message);
}


// Equivalent method showing the `release' call added by ARC.
if ([self shouldLogMessage]) {
    NSString *message = [[NSString alloc] initWithFormat:@"I am object, %p", self];
    NSLog(@"message = %@", message);
    [message release]; ///< Added by ARC
}


// Examples showing what ARC does depending on the method name.
- (EOCPerson*)newPerson {
    EOCPerson *person = [[EOCPerson alloc] init];
    return person;
    /**
     * The method name begins with `new’, and since `person’ 
     * already has an unbalanced +1 reference count from the 
     * `alloc’, no retains, releases or autoreleases are 
     * required when returning.
     */
}

- (EOCPerson*)somePerson {
    EOCPerson *person = [[EOCPerson alloc] init];
    return person;
    /**
     * The method name does not begin with one of the "owning" 
     * prefixes, therefore ARC will add an autorelease when 
     * returning `person’.
     * The equivalent manual reference counting statement is:
     *   return [person autorelease];
     */
}

- (void)doSomething {
    EOCPerson *personOne = [self newPerson];
    // …

    EOCPerson *personTwo = [self somePerson];
    // …

    /**
     * At this point, `personOne’ and `personTwo’ go out of 
     * scope, therefore ARC needs to clean them up as required. 
     * - `personOne’ was returned as owned by this block of 
         code, so it needs to be released.
     * - `personTwo’ was returned not owned by this block of 
         code, so it does not need to be released.
     * The equivalent manual reference counting cleanup code 
     * is:
     *    [personOne release];
     */
}


// Setting an instance variable under ARC.
// From a class where _myPerson is a strong instance variable
_myPerson = [EOCPerson personWithName:@"Bob Smith"];


// Showing what ARC adds for the above code.
EOCPerson *tmp = [EOCPerson personWithName:@"Bob Smith"];
_myPerson = [tmp retain];


// Showing the C calls emitted by ARC for returning autoreleased and retaining an autoreleased return value.
// Within EOCPerson class
+ (EOCPerson*)personWithName:(NSString*)name {
    EOCPerson *person = [[EOCPerson alloc] init];
    person.name = name;
    objc_autoreleaseReturnValue(person);
}

// Code using EOCPerson class
EOCPerson *tmp = [self somePerson];
_myPerson = objc_retainAutoreleasedReturnValue(tmp);


// Pseudocode of autoreleased return value methods
id objc_autoreleaseReturnValue(id object) {
    if (return_code_will_retain_object) {
        set_flag(object);
    } else {
        return [object autorelease];
    }
}

id objc_retainAutoreleasedReturnValue(id object) {
    if (get_flag(object)) {
        clear_flag();
    } else {
        return [object retain];
    }
}


// Illustrating memory management semantics of instance variables
@interface EOCClass : NSObject {
    id _object;
}

@implementation EOCClass
- (void)setup {
    _object = [EOCOtherClass new];
}
@end


// Showing memory management calls added by ARC in `setup' method
- (void)setup {
    id tmp = [EOCOtherClass new];
    _object = [tmp retain];
    [tmp release];
}


// Setter under manual reference counting, exhibiting typical mistake
- (void)setObject:(id)object {
    [_object release];
    _object = [object retain];
}


// Setter under ARC, guaranteed to be safe
- (void)setObject:(id)object {
    _object = object;
}


// __weak and __unsafe_unretained attributes
@interface EOCClass : NSObject {
    id __weak _weakObject;
    id __unsafe_unretained _unsafeUnretainedObject;
}


// __weak local variable
NSURL *url = [NSURL URLWithString:@"http://www.example.com/"];
EOCNetworkFetcher *fetcher = [[EOCNetworkFetcher alloc] initWithURL:url];
EOCNetworkFetcher * __weak weakFetcher = fetcher;
[fetcher startWithCompletion:^(BOOL success){
    NSLog(@"Finished fetching from %@", weakFetcher.url);
}];


// `dealloc' method under manual reference counting
- (void)dealloc {
    [_foo release];
    [_bar release];
    [super dealloc];
}


// `dealloc' method under ARC
- (void)dealloc {
    CFRelease(_coreFoundationObject);
    free(_heapAllocatedMemoryBlob);
}
