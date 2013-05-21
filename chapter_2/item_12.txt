// Item 12

// Exchanging methods
Method originalMethod = 
    class_getInstanceMethod([NSString class], 
                            @selector(lowercaseString));
Method swappedMethod = 
    class_getInstanceMethod([NSString class], 
                            @selector(uppercaseString));
method_exchangeImplementations(originalMethod, swappedMethod);


// Using the method exchanging of NSString
NSString *string = @"ThIs iS tHe StRiNg";

NSString *lowercaseString = [string lowercaseString];
NSLog(@"lowercaseString = %@", lowercaseString);
// Output: lowercaseString = THIS IS THE STRING

NSString *uppercaseString = [string uppercaseString];
NSLog(@"uppercaseString = %@", uppercaseString);
// Output: uppercaseString = this is the string


// Using a category and then swizzling methods
@interface NSString (MyAdditions)
- (NSString*)myLowercaseString;
@end

@implementation NSString (MyAdditions)
- (NSString*)myLowercaseString {
    NSString *lowercase = [self myLowercaseString];
    NSLog(@"Lowercasing string: %@ => %@", self, lowercase);
    return lowercase;
}
@end

Method originalMethod = class_getInstanceMethod([NSString class], @selector(lowercaseString));
Method swappedMethod = class_getInstanceMethod([NSString class], @selector(myLowercaseString));
method_exchangeImplementations(originalMethod, swappedMethod);

NSString *string = @"ThIs iS tHe StRiNg";
NSString *lowercaseString = [string lowercaseString];
// Output: Lowercasing string: ThIs iS tHe StRiNg => this is the string
