// Item 29

// Simple example of reference counting in action
NSMutableArray *array = [[NSMutableArray alloc] init];

NSNumber *number = [[NSNumber alloc] initWithInt:1337];
[array addObject:number];
[number release];

// do something with `array’

[array release];


// Example of using an object after you have released your reference to it. Bad idea.
NSNumber *number = [[NSNumber alloc] initWithInt:1337];
[array addObject:number];
[number release];
NSLog(@"number = %@", number);


// Setting the variable to nil after releasing so that you don't accidentally use an unsafe variable.
NSNumber *number = [[NSNumber alloc] initWithInt:1337];
[array addObject:number];
[number release];
number = nil;


// Memory management semantics of a strong setter accessor.
- (void)setFoo:(id)foo {
    [foo retain];
    [_foo release];
    _foo = foo;
}


// `stringValue' method without an autorelease - incorrect semantics as per method name.
- (NSString*)stringValue {
    NSString *str = [[NSString alloc] initWithFormat:@"I am this: %@", self];
    return str;
}


// `stringValue' method with an autorelease.
- (NSString*)stringValue {
    NSString *str = [[NSString alloc] initWithFormat:@"I am this: %@", self];
    return [str autorelease];
}


// Using a value that's returned autoreleased.
NSString *str = [self stringValue];
NSLog(@"The string is: %@", str);


// Retaining an object returned autoreleased if required.
_instanceVariable = [[self stringValue] retain];
// …
[_instanceVariable release];
