// Item 33

// Object model with potential retain cycle
#import <Foundation/Foundation.h>

@class EOCClassA;
@class EOCClassB;

@interface EOCClassA : NSObject
@property (nonatomic, strong) EOCClassB *other;
@end

@interface EOCClassB : NSObject
@property (nonatomic, strong) EOCClassA *other;
@end


// Fixing retain cycle with weak reference
#import <Foundation/Foundation.h>

@class EOCClassA;
@class EOCClassB;

@interface EOCClassA : NSObject
@property (nonatomic, strong) EOCClassB *other;
@end

@interface EOCClassB : NSObject
@property (nonatomic, unsafe_unretained) EOCClassA *other;
@end
