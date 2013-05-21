// Item 20

// Private method prefixing
#import <Foundation/Foundation.h>

@interface EOCObject : NSObject
- (void)publicMethod;
@end

@implementation EOCObject

- (void)publicMethod {
    …
}

- (void)p_privateMethod {
    …
}

@end


// Example overriding UIViewController method
#import <UIKit/UIKit.h>

@interface EOCViewController : UIViewController
@end

@implementation EOCViewController
- (void)_resetViewController {
    // Reset state and views
}
@end
