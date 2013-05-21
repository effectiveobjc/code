// Item 17

// Person class
#import <Foundation/Foundation.h>

@interface EOCPerson : NSObject

@property (nonatomic, copy, readonly) NSString *firstName;
@property (nonatomic, copy, readonly) NSString *lastName;

- (id)initWithFirstName:(NSString*)firstName 
               lastName:(NSString*)lastName;

@end

@implementation EOCPerson
- (id)initWithFirstName:(NSString*)firstName 
               lastName:(NSString*)lastName
{
    if ((self = [super init])) {
        _firstName = [firstName copy];
        _lastName = [lastName copy];
    }
    return self;
}
@end


// Description method for EOCPerson
- (NSString*)description {
    return [NSString stringWithFormat:@"<%@: %p, \"%@ %@\">", 
            [self class], self, _firstName, _lastName];
}


// Outputting description of an EOCPerson instance
EOCPerson *person = [[EOCPerson alloc] initWithFirstName:@"Bob" lastName:@"Smith"];
NSLog(@"person = %@", person);
// Output:
// person = <EOCPerson: 0x7fb249c030f0, "Bob Smith">


// Location class
#import <Foundation/Foundation.h>

@interface EOCLocation : NSObject
@property (nonatomic, copy, readonly) NSString *title;
@property (nonatomic, assign, readonly) float latitude;
@property (nonatomic, assign, readonly) float longitude;
- (id)initWithTitle:(NSString*)title latitude:(float)latitude longitude:(float)longitude;
@end

@implementation EOCLocation
- (id)initWithTitle:(NSString*)title latitude:(float)latitude longitude:(float)longitude {
	if ((self = [super init])) {
		_title = [title copy];
		_latitude = latitude;
		_longitude = longitude;
	}
	return self;
}
@end


// Description method for EOCLocation
- (NSString*)description {
    return [NSString stringWithFormat:@"<%@: %p, %@>",
            [self class], 
            self, 
            @{@"title":_title, 
              @"latitude":@(_latitude), 
              @"longitude":@(_longitude)}
           ];
}


// Outputting description of an EOCLocation instance
EOCPerson *person = [[EOCPerson alloc] initWithFirstName:@"Bob" lastName:@"Smith"];
NSLog(@"person = %@", person);
// Breakpoint here


// description vs debugDescription
- (NSString*)description {
    return [NSString stringWithFormat:@"%@ %@", _firstName, _lastName];
}

- (NSString*)debugDescription {
    return [NSString stringWithFormat:@"<%@: %p, \"%@ %@\">", 
            [self class], self, _firstName, _lastName];
}


// Outputting description of an NSArray instance
NSArray *array = @[@"Effective Objective-C 2.0", @(123), @(YES)];
NSLog(@"array = %@", array);
// Breakpoint here
