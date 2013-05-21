// Item 4

// EOCAnimatedView.h
#import <UIKit/UIKit.h>

@interface EOCAnimatedView : UIView
- (void)animate;
@end

// EOCAnimatedView.m
#import "EOCAnimatedView.h"

static const NSTimeInterval kAnimationDuration = 0.3;

@implementation EOCAnimatedView
- (void)animate {
    [UIView animateWithDuration:kAnimationDuration
                     animations:^(){
                         // Perform animations
                     }];
}
@end


// In the header file
extern NSString *const StringConstant;

// In the implementation file
NSString *const StringConstant = @"VALUE";


// EOCLoginManager.h
#import <Foundation/Foundation.h>

extern NSString *const EOCLoginManagerDidLoginNotification;

@interface EOCLoginManager : NSObject
- (void)login;
@end

// EOCLoginManager.m
#import "EOCLoginManager.h"

NSString *const EOCLoginManagerDidLoginNotification = @"EOCLoginManagerDidLoginNotification";

@implementation EOCLoginManager

- (void)login {
    // Perform login asynchronously, then call `p_didLogin`.
}

- (void)p_didLogin {
    [[NSNotificationCenter defaultCenter] 
      postNotificationName:EOCLoginManagerDidLoginNotification
                    object:nil];
}

@end


// EOCAnimatedView.h
extern const NSTimeInterval EOCAnimatedViewAnimationDuration;

// EOCAnimatedView.m
const NSTimeInterval EOCAnimatedViewAnimationDuration = 0.3;
