// Item 13

// Button delegate example
@class Button;
@protocol ButtonDelegate <NSObject>
@optional
- (void)buttonWasClicked:(Button*)button;
- (void)buttonWasDoubleClicked:(Button*)button;
@end

@interface Button : NSObject
@property (nonatomic, weak) id <ButtonDelegate> delegate;
@end

@implementation Button
...
- (void)buttonWasClicked {
    if ([_delegate respondsToSelector:@selector(buttonWasClicked:)]) {
        [_delegate buttonWasClicked:self];
    }
    // Do button related things, e.g. change highlight color
}

- (void)buttonWasDoubleClicked {
    if ([_delegate respondsToSelector:@selector(buttonWasDoubleClicked:)]) {
        [_delegate buttonWasDoubleClicked:self];
    }
    // Do button related things, e.g. change highlight color
}
...
@end


// Inspecting class hierarchy
NSMutableDictionary *dict = [NSMutableDictionary new];
[dict isMemberOfClass:[NSDictionary class]]; ///< NO
[dict isMemberOfClass:[NSMutableDictionary class]]; ///< YES
[dict isKindOfClass:[NSDictionary class]]; ///< YES
[dict isKindOfClass:[NSArray class]]; ///< NO


// Example of using inspection of types
- (NSString*)commaSeparatedStringFromObjects:(NSArray*)array {
    NSMutableString *string = [NSMutableString new];
    for (id object in array) {
        if ([object isKindOfClass:[NSString class]]) {
            [string appendFormat:@"%@,", object];
        } else if ([object isKindOfClass:[NSNumber class]]) {
            [string appendFormat:@"%d,", [object intValue]];
        } else if ([object isKindOfClass:[NSData class]]) {
            NSString *base64Encoded = /* base64 encode the data */;
            [string appendFormat:@"%@,", base64Encoded];
        } else {
            // Type not supported
        }
    }
    return string;
}
