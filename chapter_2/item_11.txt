// Item 11

// `resolveInstanceMethod' example
id autoDictionaryGetter(id self, SEL _cmd);
void autoDictionarySetter(id self, SEL _cmd, id value);

+ (BOOL)resolveInstanceMethod:(SEL)selector {
    NSString *selectorString = NSStringFromSelector(selector);
    if (/* selector is from a @dynamic property */) {
        if ([selectorString hasPrefix:@"set"]) {
            class_addMethod(self, selector, (IMP)autoDictionarySetter, "v@:@");
        } else {
            class_addMethod(self, selector, (IMP)autoDictionaryGetter, "@@:");
        }
        return YES;
    }
    return [super resolveInstanceMethod:selector];
}


// Full "AutoDictionary" example
#import <Foundation/Foundation.h>

@interface AutoDictionary : NSObject
@property (nonatomic, strong) NSString *string;
@property (nonatomic, strong) NSNumber *number;
@property (nonatomic, strong) NSDate *date;
@property (nonatomic, strong) id opaqueObject;
@end

#import "AutoDictionary.h"
#import <objc/runtime.h>

id autoDictionaryGetter(id self, SEL _cmd) {
    // Get the backing store from the object
    AutoDictionary *typedSelf = (AutoDictionary*)self;
    NSMutableDictionary *backingStore = typedSelf.backingStore;
    
    // The key is simply the selector name
    NSString *key = NSStringFromSelector(_cmd);
    
    // Return the value
    return [backingStore objectForKey:key];
}

void autoDictionarySetter(id self, SEL _cmd, id value) {
    // Get the backing store from the object
    AutoDictionary *typedSelf = (AutoDictionary*)self;
    NSMutableDictionary *backingStore = typedSelf.backingStore;
    
    /** The selector will be for example, "setOpaqueObject:".
     *  We need to remove the "set", ":" and lowercase the first
     *  letter of the remainder.
     */
    NSString *selectorString = NSStringFromSelector(_cmd);
    NSMutableString *key = [selectorString mutableCopy];
    
    // Remove the `:' at the end
    [key deleteCharactersInRange:NSMakeRange(key.length - 1, 1)];
    
    // Remove the `set' prefix
    [key deleteCharactersInRange:NSMakeRange(0, 3)];
    
    // Lowercase the first character
    NSString *lowercaseFirstChar = [[key substringToIndex:1] lowercaseString];
    [key replaceCharactersInRange:NSMakeRange(0, 1) withString:lowercaseFirstChar];
    
    if (value) {
        [backingStore setObject:value forKey:key];
    } else {
        [backingStore removeObjectForKey:key];
    }
}

@interface AutoDictionary ()
@property (nonatomic, strong) NSMutableDictionary *backingStore;
@end

@implementation AutoDictionary
@dynamic string, number, date, opaqueObject;

- (id)init {
    if ((self = [super init])) {
        _backingStore = [NSMutableDictionary new];
    }
    return self;
}

+ (BOOL)resolveInstanceMethod:(SEL)selector {
    NSString *selectorString = NSStringFromSelector(selector);
    if ([selectorString hasPrefix:@"set"]) {
        class_addMethod(self, selector, (IMP)autoDictionarySetter, "v@:@");
    } else {
        class_addMethod(self, selector, (IMP)autoDictionaryGetter, "@@:");
    }
    return YES;
}

@end


// Using AutoDictionary
AutoDictionary *dict = [AutoDictionary new];
dict.date = [NSDate dateWithTimeIntervalSince1970:475372800];
NSLog(@"dict.date = %@", dict.date);
// Output: dict.date = 1985-01-24 00:00:00 +0000
