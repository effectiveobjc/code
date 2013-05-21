// Item 6

// Person interface
@interface Person : NSObject {
@public
    NSDate *_dateOfBirth;
    NSString *_firstName;
    NSString *_lastName;
@private
    NSString *_someInternalData;
}


// Person interface with properties
@interface Person : NSObject
@property NSString *firstName;
@property NSString *lastName;
@end


// Person interface what properties equate to
@interface Person : NSObject
- (NSString*)firstName;
- (void)setFirstName:(NSString*)firstName;
- (NSString*)lastName;
- (void)setLastName:(NSString*) lastName;
@end

Person *aPerson = [Person new];

aPerson.firstName = @"Bob"; // Same as:
[aPerson setFirstName:@"Bob"];

NSString *lastName = aPerson.lastName; // Same as:
NSString *lastName = [aPerson lastName];


// Synthesizing properties
@implementation Person
@synthesize firstName = _myFirstName;
@synthesize lastName = _myLastName;
@end


// NSManagedObject example for @dynamic
@interface Person : NSManagedObject
@property NSString *firstName;
@property NSString *lastName;
@end

@implementation Person
@dynamic firstName, lastName;
@end

