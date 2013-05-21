// Item 35

// @autoreleasepool block in main.m
int main(int argc, char *argv[]) {
    @autoreleasepool {
        return UIApplicationMain(argc, argv, nil, @"EOCAppDelegate");
    }
}


// Nested pools
@autoreleasepool {
    NSString *string = [NSString stringWithFormat:@"1 = %i", 1];
    @autoreleasepool {
        NSNumber *number = [NSNumber numberWithInt:1];
    }
}


// Loop which may cause a lot of autoreleases to happen
for (int i = 0; i < 100000; i++) {
    [self doSomethingWithInt:i];
}


// Another loop that might create lots of autoreleased objects
NSArray *databaseRecords = …;
NSMutableArray *people = [NSMutableArray new];
for (NSDictionary *record in databaseRecords) {
    EOCPerson *person = [[EOCPerson alloc] initWithRecord:record];
    [people addObject:person];
}


// Reducing high memory waterline with appropriately places @autoreleasepool
NSArray *databaseRecords = …;
NSMutableArray *people = [NSMutableArray new];
for (NSDictionary *record in databaseRecords) {
    @autoreleasepool {
        EOCPerson *person = [[EOCPerson alloc] initWithRecord:record];
        [people addObject:person];
    }
}


// Old autorelease pool style
NSArray *databaseRecords = …;
NSMutableArray *people = [NSMutableArray new];
int i = 0;

NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
for (NSDictionary *record in databaseRecords) {
    EOCPerson *person = [[EOCPerson alloc] initWithRecord:record];
    [people addObject:person];

    // Drain the pool only every 10 cycles
    if (++i == 10) {
        [pool drain];
    }
}

// Also drain at the end incase the loop is not a multiple of 10
[pool drain];


// Problem with old style pools
NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
id object = [self createObject];
[pool drain];
[self useObject:object];


// Problem cannot happen with new pools
@autoreleasepool {
    id object = [self createObject];
}
[self useObject:object];
