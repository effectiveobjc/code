// Item 16

// Rectangle class
#import <Foundation/Foundation.h>

@interface EOCRectangle : NSObject
@property (nonatomic, assign, readonly) float width;
@property (nonatomic, assign, readonly) float height;
@end


// Designated initialiser
- (id)initWithWidth:(float)width 
          andHeight:(float)height
{
    if ((self = [super init])) {
        _width = width;
        _height = height;
    }
    return self;
}


// Overriding super class's designated initialiser
- (id)init {
    return [self initWithWidth:5.0f andHeight:10.0f];
}


// Square class with designated initialiser different to its super class
#import "EOCRectangle.h"

@interface EOCSquare : EOCRectangle
- (id)initWithDimension:(float)dimension;
@end

@implementation EOCSquare

- (id)initWithDimension:(float)dimension {
    return [super initWithWidth:dimension andHeight:dimension];
}

@end


// Overriding super class's designated initialiser in EOCSquare
- (id)initWithWidth:(float)width andHeight:(float)height {
    float dimension = MAX(width, height);
    return [self initWithDimension:dimension];
}


// Throwing exception in overridden superclass designated initialiser
- (id)initWithWidth:(float)width andHeight:(float)height {
    @throw [NSException
        exceptionWithName:NSInternalInconsistencyException
                   reason:@"Must use initWithDimension: instead."
                 userInfo:nil];
}


// Overriding super class's super class designated initialiser
- (id)init {
    return [self initWithDimension:5.0f];
}


// Adding another designated initialiser from NSCoding
#import <Foundation/Foundation.h>

@interface EOCRectangle : NSObject <NSCoding>
@property (nonatomic, assign, readonly) float width;
@property (nonatomic, assign, readonly) float height;
- (id)initWithWidth:(float)width 
          andHeight:(float)height;
@end

@implementation EOCRectangle

// Designated initialiser
- (id)initWithWidth:(float)width 
          andHeight:(float)height
{
    if ((self = [super init])) {
        _width = width;
        _height = height;
    }
    return self;
}

// Super-class’s designated initialiser
- (id)init {
    return [self initWithWidth:5.0f andHeight:10.0f];
}

// Initialiser from NSCoding
- (id)initWithCoder:(NSCoder*)decoder {
    // Call through to super’s designated initialiser
    if ((self = [super init])) {
        _width = [decoder decodeFloatForKey:@"width"];
        _height = [decoder decodeFloatForKey:@"height"];
    }
}

@end


// EOCSquare after implementing NSCoding
#import "EOCRectangle.h"

@interface EOCSquare : EOCRectangle
- (id)initWithDimension:(float)dimension;
@end

@implementation EOCSquare

// Designated initialiser
- (id)initWithDimension:(float)dimension {
    return [super initWithWidth:dimension andHeight:dimension];
}

// Super class designated initialiser
- (id)initWithWidth:(float)width andHeight:(float)height {
    float dimension = MAX(width, height);
    return [self initWithDimension:dimension];
}

// NSCoding designated initialiser
- (id)initWithCoder:(NSCoder*)decoder {
    if ((self = [super initWithCoder:decoder])) {
        // EOCSquare’s specific initialiser
    }
}

@end
