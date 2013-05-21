// Item 7

// Examples of equality and non-equality
NSString *foo = @"Badger 123";
NSString *bar = [NSString stringWithFormat:@"Badger %i", 123];
BOOL equalA = (foo == bar); //< equalA = NO
BOOL equalB = [foo isEqual:bar]; //< equalB = YES
BOOL equalC = [foo isEqualToString:bar]; //< equalC = YES


// Equality methods
- (BOOL)isEqual:(id)object;
- (NSUInteger)hash;


// Person interface
@interface Person : NSObject
@property (nonatomic, copy) NSString *firstName;
@property (nonatomic, copy) NSString *lastName;
@property (nonatomic, assign) NSUInteger age;
@end



// Equality method
- (BOOL)isEqual:(id)object {
    if (self == object) return YES;
    if ([self class] != [object class]) return NO;
    
    Person *otherPerson = (Person*)object;
    if (![_firstName isEqualToString:otherPerson.firstName])
        return NO;
    if (![_lastName isEqualToString:otherPerson.lastName])
        return NO;
    if (_age != otherPerson.age)
        return NO;
    return YES;
}


// Super simple hash method
- (NSUInteger)hash {
    return 1337;
}


// Hash method hijacking NSString's hash method
- (NSUInteger)hash {
    NSString *stringToHash = [NSString stringWithFormat:@"%@:%@:%i", _firstName, _lastName, _age];
    return [stringToHash hash];
}


// Hash method by building up from individual values
- (NSUInteger)hash {
    NSUInteger firstNameHash = [_firstName hash];
    NSUInteger lastNameHash = [_lastName hash];
    NSUInteger ageHash = _age;
    return firstNameHash ^ lastNameHash ^ ageHash;
}



// Class specific equality methods
- (BOOL)isEqualToPerson:(Person*)object {
    if (self == object) return YES;
    
    Person *otherPerson = (Person*)object;
    if (![_firstName isEqualToString:otherPerson.firstName])
        return NO;
    if (![_lastName isEqualToString:otherPerson.lastName])
        return NO;
    if (_age != otherPerson.age)
        return NO;
    return YES;
}

- (BOOL)isEqual:(id)object {
    if ([self class] == [object class]) {
        return [self isEqualToPerson:(Person*)object];
    } else {
        return [super isEqual:object];
    }
}



// Example of mutable objects in collection problem
NSMutableSet *set = [NSMutableSet new];

NSMutableArray *arrayA = [@[@1, @2] mutableCopy];
[set addObject:arrayA];
NSLog(@"set = %@", set);
// Output: set = {((1,2))}

So the set contains 1 object - an array with 2 objects in it. Now add another array which contains equal objects in the same order, such that the array already in the set and the new one are equal:

NSMutableArray *arrayB = [@[@1, @2] mutableCopy];
[set addObject:arrayB];
NSLog(@"set = %@", set);
// Output: set = {((1,2))}

We see that the set still contains just a single object, since the object added is equal to the object already in there. Now we add another array to the set, but this time one that is not equal to the array already in the set:

NSMutableArray *arrayC = [@[@1] mutableCopy];
[set addObject:arrayC];
NSLog(@"set = %@", set);
// Output: set = {((1),(1,2))}

As expected, the set now contains two arrays. The original one and the new one, since arrayC does not equal the one already in the set. Finally we mutate arrayC to be equal to the other array already in the set:

[arrayC addObject:@2];
NSLog(@"set = %@", set);
// Output: set = {((1,2),(1,2))}

Ah oh dear, there’s now two arrays in the set which are equal to each other! A set is not meant to allow this, but it has been unable to maintain its semantics because we’ve mutated one of the objects that was already in the set. What’s even more awkward is if the set is then copied:

NSSet *setB = [set copy];
NSLog(@"setB = %@", setB);
// Output: setB = {((1,2))}
