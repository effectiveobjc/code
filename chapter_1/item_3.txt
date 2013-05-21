// Item 3

NSString *someString = @"Effective Objective-C 2.0";

NSNumber *someNumber = [NSNumber numberWithInt:1];
NSNumber *someNumber = @1;

NSNumber *intNumber = @1;
NSNumber *floatNumber = @2.5f;
NSNumber *doubleNumber = @3.14159;
NSNumber *boolNumber = @YES;
NSNumber *charNumber = @‘a’;

int x = 5;
float y = 6.32f;
NSNumber *expressionNumber = @(x * y);

NSArray *animals = [NSArray arrayWithObjects:@"cat", @"dog", @"mouse", @"badger", nil];
NSArray *animals = @[@"cat", @"dog", @"mouse", @"badger"];

NSString *dog = [animals objectAtIndex:1];
NSString *dog = animals[1];

id object1 = /* … /*;
id object2 = /* … /*;
id object3 = /* … /*;

NSArray *arrayA = [NSArray arrayWithObjects:object1, object2, object3, nil];
NSArray *arrayB = @[object1, object2, object3];

NSDictionary *personData = 
    [NSDictionary dictionaryWithObjectsAndKeys:
        @"Matt", @"firstName", 
        @"Galloway", @"lastName", 
        [NSNumber numberWithInt:28], @"age", 
        nil];
NSDictionary *personData = 
    @{@"firstName" : @"Matt", 
      @"lastName" : @"Galloway", 
      @"age" : @28};

NSString *lastName = [personData objectForKey:@"lastName"];
NSString *lastName = personData[@"lastName"];

[mutableArray replaceObjectAtIndex:1 withObject:@"dog"];
[mutableDictionary setObject:@"Galloway" forKey:@"lastName"];
mutableArray[1] = @"dog";
mutableDictionary[@"lastName"] = @"Galloway";

NSMutableArray *mutable = [@[@1, @2, @3, @4, @5] mutableCopy];
