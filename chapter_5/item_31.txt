// Item 31

// Releasing CF objects and removing observer in `dealloc'
- (void)dealloc {
    CFRelease(coreFoundationObject);
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}


// Class for connecting to a server, which will use a scarce system resource - file descriptor, etc
#import <Foundation/Foundation.h>

@interface EOCServerConnection : NSObject
- (void)open:(NSString*)address;
- (void)close;
@end


// `close' and `dealloc' methods
- (void)close {
    /* clean up resources */
    _closed = YES;
}

- (void)dealloc {
    if (!_closed) {
        NSLog(@"ERROR: close was not called before object was deallocated!");
        [self close];
    }
}
