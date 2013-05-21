// Item 18

// EOCPointOfInterest object
#import <Foundation/Foundation.h>

@interface EOCPointOfInterest : NSObject

@property (nonatomic, copy) NSString *identifier;
@property (nonatomic, copy) NSString *title;
@property (nonatomic, assign) float latitude;
@property (nonatomic, assign) float longitude;

- (id)initWithIdentifier:(NSString*)identifier
                   title:(NSString*)title
                latitude:(float)latitude
               longitude:(float)longitude;

@end


// Making everything readonly
#import <Foundation/Foundation.h>

@interface EOCPointOfInterest : NSObject

@property (nonatomic, copy, readonly) NSString *identifier;
@property (nonatomic, copy, readonly) NSString *title;
@property (nonatomic, assign, readonly) float latitude;
@property (nonatomic, assign, readonly) float longitude;

- (id)initWithIdentifier:(NSString*)identifier
                   title:(NSString*)title
                latitude:(float)latitude
               longitude:(float)longitude;

@end


// Overriding the property scope with the class continuation category
#import "EOCPointOfInterest.h"

@interface EOCPointOfInterest ()
@property (nonatomic, copy, readwrite) NSString *identifier;
@property (nonatomic, copy, readwrite) NSString *title;
@property (nonatomic, assign, readwrite) float latitude;
@property (nonatomic, assign, readwrite) float longitude;
@end

@implementation EOCPointOfInterest
…
@end


// Person class
// EOCPerson.h
#import <Foundation/Foundation.h>

@interface EOCPerson : NSObject

@property (nonatomic, copy, readonly) NSString *firstName;
@property (nonatomic, copy, readonly) NSString *lastName;
@property (nonatomic, strong, readonly) NSSet *friends;

- (id)initWithFirstName:(NSString*)firstName
            andLastName:(NSString*)lastName;
- (void)addFriend:(EOCPerson*)person;
- (void)removeFriend:(EOCPerson*)person;

@end

// EOCPerson.m
#import "EOCPerson.h"

@interface EOCPerson ()
@property (nonatomic, copy, readwrite) NSString *firstName;
@property (nonatomic, copy, readwrite) NSString *lastName;
@end

@implementation EOCPerson {
    NSMutableSet *_internalFriends
}

- (NSSet*)friends {
    return [_internalFriends copy];
}

- (void)addFriend:(EOCPerson*)person {
    [_internalFriends addObject:person];
}

- (void)removeFriend:(EOCPerson*)person {
    [_internalFriends removeObject:person];
}

- (id)initWithFirstName:(NSString*)firstName
            andLastName:(NSString*)lastName {
    if ((self = [super init])) {
        _firstName = firstName;
        _lastName = lastName;
        _internalFriends = [NSMutableSet new];
    }
    return self;
}

@end


// Introspection of returned class. Not to be done!
EOCPerson *person = …;
NSSet *friends = person.friends;
if ([friends isKindOfClass:[NSMutableSet class]]) {
    NSMutableSet *mutableFriends = (NSMutableSet*)friends;
    /* mutate the set */
}
