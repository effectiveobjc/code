// Item 22

// Person object
#import <Foundation/Foundation.h>

@interface EOCPerson : NSObject <NSCopying>

@property (nonatomic, copy, readonly) NSString *firstName;
@property (nonatomic, copy, readonly) NSString *lastName;

- (id)initWithFirstName:(NSString*)firstName 
            andLastName:(NSString*)lastName;

@end


// NSCopying implementation
- (id)copyWithZone:(NSZone*)zone {
    Person *copy = [[[self class] allocWithZone:zone] 
                    initWithFirstName:_firstName 
                          andLastName:_lastName];
    return copy;
}


// Person class with friends set, implementing NSCopying
#import <Foundation/Foundation.h>

@interface EOCPerson : NSObject <NSCopying>

@property (nonatomic, copy, readonly) NSString *firstName;
@property (nonatomic, copy, readonly) NSString *lastName;

- (id)initWithFirstName:(NSString*)firstName 
            andLastName:(NSString*)lastName;

- (void)addFriend:(EOCPerson*)person;
- (void)removeFriend:(EOCPerson*)person;

@end

@implementation EOCPerson {
    NSMutableSet *_friends;
}

- (id)initWithFirstName:(NSString*)firstName 
            andLastName:(NSString*)lastName {
    if ((self = [super init])) {
        _firstName = [firstName copy];
        _lastName = [lastName copy];
        _friends = [NSMutableSet new];
    }
    return self;
}

- (void)addFriend:(EOCPerson*)person {
    [_friends addObject:person];
}

- (void)removeFriend:(EOCPerson*)person {
    [_friends removeObject:person];
}

- (id)copyWithZone:(NSZone*)zone {
    EOCPerson *copy = [[[self class] allocWithZone:zone] 
                       initWithFirstName:_firstName 
                             andLastName:_lastName];
    copy->_friends = [_friends mutableCopy];
    return copy;
}

@end


// Deep copy of EOCPerson object
- (id)deepCopy {
    EOCPerson *copy = [[[self class] allocWithZone:zone] 
                       initWithFirstName:_firstName 
                             andLastName:_lastName];
    copy->_friends = [[NSSet alloc] initWithSet:_friends 
                                      copyItems:YES];
    return copy;
}
