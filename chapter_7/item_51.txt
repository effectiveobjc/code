// Item 51

// Simple load example
#import <Foundation/Foundation.h>

#import "EOCClassA.h" //< From the same library

@interface EOCClassB : NSObject
@end

@implementation EOCClassB
+ (void)load {
    NSLog(@"Loading EOCClassB");
    EOCClassA *object = [EOCClassA new];
    // Use `object’
}
@end


// Simple initialize example
#import <Foundation/Foundation.h>

@interface EOCBaseClass : NSObject
@end

@implementation EOCBaseClass
+ (void)initialize {
    NSLog(@"%@ initialize", self);
}
@end

@interface EOCSubClass : EOCBaseClass
@end

@implementation EOCSubClass
@end


// Checking the class in initialize
+ (void)initialize {
    if (self == [EOCBaseClass class]) {
        NSLog(@"%@ initialized", self);
    }
}


// Chicken and egg
#import <Foundation/Foundation.h>

static id EOCClassAInternalData;
@interface EOCClassA : NSObject
@end

static id EOCClassBInternalData;
@interface EOCClassB : NSObject
@end

@implementation EOCClassA

+ (void)initialize {
    if (self == [EOCClassA class]) {
        [EOCClassB doSomethingThatUsesItsInternalData];
        EOCClassAInternalData = [self setupInternalData];
    }
}

@end

@implementation EOCClassB

+ (void)initialize {
    if (self == [EOCClassB class]) {
        [EOCClassA doSomethingThatUsesItsInternalData];
        EOCClassBInternalData = [self setupInternalData];
    }
}

@end


// Lean initialize
// EOCClass.h
#import <Foundation/Foundation.h>

@interface EOCClass : NSObject
@end

// EOCClass.m
#import "EOCClass.h"

static const int kInterval = 10;
static NSMutableArray *kSomeObjects;

@implementation EOCClass
+ (void)initialize {
    if (self == [EOCClass class]) {
        kSomeObjects = [NSMutableArray new];
    }
}
@end
