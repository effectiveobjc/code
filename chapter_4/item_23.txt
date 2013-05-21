// Item 23

// Network fetcher delegate
@protocol EOCNetworkFetcherDelegate
- (void)networkFetcher:(EOCNetworkFetcher*)fetcher 
        didReceiveData:(NSData*)data;
- (void)networkFetcher:(EOCNetworkFetcher*)fetcher
        didFailWithError:(NSError*)error;
@end


// Property for delegate
@interface EOCNetworkFetcher : NSObject
@property (nonatomic, weak) id <EOCNetworkFetcherDelegate> delegate;
@end


// Implementing the delegate
@implementation EOCDataModel () <EOCNetworkFetcherDelegate>
@end

@implementation EOCDataModel
- (void)networkFetcher:(EOCNetworkFetcher*)fetcher 
        didReceiveData:(NSData*)data {
    /* Handle data */
}
- (void)networkFetcher:(EOCNetworkFetcher*)fetcher
        didFailWithError:(NSError*)error {
    /* Handle error */
}
@end


// Optional methods
@protocol EOCNetworkFetcherDelegate
@optional
- (void)networkFetcher:(EOCNetworkFetcher*)fetcher 
        didReceiveData:(NSData*)data;
- (void)networkFetcher:(NetworkFetcher*)fetcher
        didFailWithError:(EOCNSError*)error;
@end


// Checking if the delegate responds to selector
NSData *data = /* data obtained from network */;
if ([_delegate respondsToSelector:@selector(networkFetcher:didReceiveData:)]) {
    [_delegate networkFetcher:self didReceiveData:data];
}


// Checking which network fetcher caused the callback
- (void)networkFetcher:(EOCNetworkFetcher*)fetcher 
        didReceiveData:(NSData*)data {
    if (fetcher == _myFetcherA) {
        /* Handle data */
    } else if (fetcher == _myFetcherB) {
        /* Handle data */
    }
}


// Extended protocol
@protocol EOCNetworkFetcherDelegate
@optional
- (void)networkFetcher:(EOCNetworkFetcher*)fetcher 
        didReceiveData:(NSData*)data;
- (void)networkFetcher:(EOCNetworkFetcher*)fetcher
        didFailWithError:(NSError*)error;
- (void)networkFetcher:(EOCNetworkFetcher*)fetcher
   didUpdateProgressTo:(float)progress;
@end


// Bitfield struct
struct data {
    unsigned int fieldA : 8;
    unsigned int fieldB : 4;
    unsigned int fieldC : 2;
    unsigned int fieldD : 1;
};


// Flags to indicate what methods the delegate responds to
@interface EOCNetworkFetcher () {
    struct {
        unsigned int didReceiveData      : 1;
        unsigned int didFailWithError    : 1;
        unsigned int didUpdateProgressTo : 1;
    } _delegateFlags;
}
@end


// Setting and checking flags
// Set flag
_delegateFlags.didReceiveData = 1;

// Check flag
if (_delegateFlags.didReceiveData) {
    // Yes, flag set
}


// Setting delegate and setting flags
- (void)setDelegate:(id<EOCNetworkFetcher)delegate {
    _delegate = delegate;
    _delegateFlags.didReceiveData = [delegate respondsToSelector:@selector(networkFetcher:didReceiveData:)];
    _delegateFlags.didFailWithError = [delegate respondsToSelector:@selector(networkFetcher:didFailWithError:)];
    _delegateFlags.didUpdateProgressTo = [delegate respondsToSelector:@selector(networkFetcher:didUpdateProgressTo:)];
}


// Testing flags
if (_delegateFlags.didUpdateProgressTo) {
    [_delegate networkFetcher:self didUpdateProgressTo:currentProgress];
}
