// Item 27

// Secret instance variable
#import <Foundation/Foundation.h>

@class EOCSuperSecretClass

@interface EOCClass : NSObject {
@private
    EOCSuperSecretClass *_secretInstance;
}
@end


// Using class continuation category to hide secret instance variable
// EOCClass.h
#import <Foundation/Foundation.h>
@interface EOCClass : NSObject
@end

// EOCClass.m
#import "EOCClass.h"
#import "EOCSuperSecretClass.h"

@interface EOCClass () {
    EOCSuperSecretClass *_secretInstance;
}
@end

@implementation EOCClass
// Methods here
@end


// C++ instance variable
// EOCClass.h
#import <Foundation/Foundation.h>

#include "SomeCppClass.h"

@interface EOCClass : NSObject {
@private
    SomeCppClass _cppClass;
}
@end


// Forward declaring C++ class
// EOCClass.h
#import <Foundation/Foundation.h>

class SomeCppClass;

@interface EOCClass : NSObject {
@private
    SomeCppClass *_cppClass;
}
@end


// Using class continuation category to hide C++ class
// EOCClass.h
#import <Foundation/Foundation.h>

@interface EOCClass : NSObject
@end

// EOCClass.mm
#import "EOCClass.h"
#include "SomeCppClass.h"

@interface EOCClass () {
    SomeCppClass _cppClass;
}
@end

@implementation EOCClass
@end


// Readonly properties on class
#import <Foundation/Foundation.h>

@interface EOCPerson : NSObject
@property (nonatomic, copy, readonly) NSString *firstName;
@property (nonatomic, copy, readonly) NSString *lastName;

- (id)initWithFirstName:(NSString*)firstName
               lastName:(NSString*)lastName;
@end


// Redeclaring properties as readwrite
@interface EOCPerson ()
@property (nonatomic, copy, readwrite) NSString *firstName;
@property (nonatomic, copy, readwrite) NSString *lastName;
@end


// Secret delegate in public interface
#import <Foundation/Foundation.h>
#import "EOCSecretDelegate.h"

@interface EOCPerson : NSObject <EOCSecretDelegate>
@property (nonatomic, copy, readonly) NSString *firstName;
@property (nonatomic, copy, readonly) NSString *lastName;

- (id)initWithFirstName:(NSString*)firstName
               lastName:(NSString*)lastName;
@end


// Moving declaration of conformance to protocol to class continuation category
#import "EOCPerson.h"
#import "EOCSecretDelegate.h"

@interface EOCPerson () <EOCSecretDelegate>
@end

@implementation EOCPerson
…
@end
