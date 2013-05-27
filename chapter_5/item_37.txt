// Item 37

// Don't do this
while ([object retainCount]) {
    [object release];
}


// Large "retain counts" of some objects (singletons)
NSString *string = @"Some string";
NSLog(@"string retainCount = %lu", [string retainCount]);

NSNumber *numberI = @1;
NSLog(@"numberI retainCount = %lu", [numberI retainCount]);

NSNumber *numberF = @3.141f;
NSLog(@"numberF retainCount = %lu", [numberF retainCount]);


// Showing retain count might not necessarily be what you expect
id object = [self createObject];
[opaqueObject doSomethingWithObject:object];
NSLog(@"retainCount = %lu", [object retainCount]);
