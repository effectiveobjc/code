// Item 52

// Example showing retain cycle
#import <Foundation/Foundation.h>

@interface EOCClass : NSObject
- (void)startPolling;
- (void)stopPolling;
@end

@implementation EOCClass {
    NSTimer *_pollTimer;
}

- (id)init {
    return [super init];
}

- (void)dealloc {
    [_pollTimer invalidate];
}

- (void)stopPolling {
    [_pollTimer invalidate];
    _pollTimer = nil;
}

- (void)startPolling {
    _pollTimer = 
    [NSTimer scheduledTimerWithTimeInterval:5.0
                                     target:self
                                   selector:@selector(p_doPoll)
                                   userInfo:nil
                                    repeats:YES];
}

- (void)p_doPoll {
    // Poll the resource
}

@end


// Block support for NSTimer
#import <Foundation/Foundation.h>

@interface NSTimer (EOCBlocksSupport)

+ (void)eoc_scheduledTimerWithTimeInterval:(NSTimeInterval)interval
                                 block:(void(^)())block
                               repeats:(BOOL)repeats;

@end

@implementation NSTimer (EOCBlocksSupport)

+ (void)eoc_scheduledTimerWithTimeInterval:(NSTimeInterval)interval
                                 block:(void(^)())block
                               repeats:(BOOL)repeats
{
    return [self scheduledTimerWithTimeInterval:interval
                                         target:self
                           selector:@selector(eoc_blockInvoke:)
                                       userInfo:[block copy]
                                        repeats:repeats];
}

+ (void)eoc_blockInvoke:(NSTimer*)timer {
    void (^block)() = timer.userInfo;
    if (block) {
        block();
    }
}

@end


// Changing to use the block - still a cycle
- (void)startPolling {
    _pollTimer = 
    [NSTimer eoc_scheduledTimerWithTimeInterval:5.0
                                          block:^{
                                              [self p_doPoll];
                                                 }
                                        repeats:YES];
}


// No cycle using weak references
- (void)startPolling {
    __weak EOCClass *weakSelf = self;
    _pollTimer = 
    [NSTimer eoc_scheduledTimerWithTimeInterval:5.0
                                          block:^{
                               EOCClass *strongSelf = weakSelf;
                               [strongSelf p_doPoll];
                                                 }
                                        repeats:YES];
}
