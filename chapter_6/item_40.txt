// Item 40

// Network fetcher class using delegation
#import <Foundation/Foundation.h>

@class EOCNetworkFetcher;
@protocol EOCNetworkFetcherDelegate <NSObject>
- (void)networkFetcher:(EOCNetworkFetcher*)networkFetcher didFinishWithData:(NSData*)data;
@end

@interface EOCNetworkFetcher : NSObject
@property (nonatomic, weak) id <EOCNetworkFetcherDelegate> delegate;
- (id)initWithURL:(NSURL*)url;
- (void)start;
@end


// Using the network fetcher class with delegation
- (void)fetchFooData {
    NSURL *url = [[NSURL alloc] initWithString:@"http://www.example.com/foo.dat"];
    EOCNetworkFetcher *fetcher = [[EOCNetworkFetcher alloc] initWithURL:url];
    fetcher.delegate = self;
    [fetcher start];
}

// …

- (void)networkFetcher:(EOCNetworkFetcher*)networkFetcher didFinishWithData:(NSData*)data {
    _fetchedFooData = data;
}


// Simplifying with a completion handler block
#import <Foundation/Foundation.h>

typedef void(^EOCNetworkFetcherCompletionHandler)(NSData *data);

@interface EOCNetworkFetcher : NSObject
- (id)initWithURL:(NSURL*)url;
- (void)startWithCompletionHandler:(EOCNetworkFetcherCompletionHandler)handler;
@end


// Using the completion handler block API - simpler
- (void)fetchFooData {
    NSURL *url = [[NSURL alloc] initWithString:@"http://www.example.com/foo.dat"];
    EOCNetworkFetcher *fetcher = [[EOCNetworkFetcher alloc] initWithURL:url];
    [fetcher startWithCompletionHandler:^(NSData *data){
        _fetchedFooData = data;
    }];
}


// Multiple fetchers with delegation
- (void)fetchFooData {
    NSURL *url = [[NSURL alloc] initWithString:@"http://www.example.com/foo.dat"];
    _fooFetcher = [[EOCNetworkFetcher alloc] initWithURL:url];
    _fooFetcher.delegate = self;
    [_fooFetcher start];
}

- (void)fetchBarData {
    NSURL *url = [[NSURL alloc] initWithString:@"http://www.example.com/bar.dat"];
    _fooFetcher = [[EOCNetworkFetcher alloc] initWithURL:url];
    _fooFetcher.delegate = self;
    [_fooFetcher start];
}

- (void)networkFetcher:(EOCNetworkFetcher*)networkFetcher didFinishWithData:(NSData*)data {
    if (networkFetcher == _fooFetcher) {
        _fetchedFooData = data;
        _fooFetcher = nil;
    } else if (networkFetcher == _barFetcher) {
        _fetchedBarData = data;
        _barFetcher = nil;
    }
    // etc
}


// Multiple fetchers with completion handler block
- (void)fetchFooData {
    NSURL *url = [[NSURL alloc] initWithString:@"http://www.example.com/foo.dat"];
    EOCNetworkFetcher *fetcher = [[EOCNetworkFetcher alloc] initWithURL:url];
    [fetcher startWithCompletionHandler:^(NSData *data){
        _fetchedFooData = data;
    }];
}

- (void)fetchBarData {
    NSURL *url = [[NSURL alloc] initWithString:@"http://www.example.com/bar.dat"];
    EOCNetworkFetcher *fetcher = [[EOCNetworkFetcher alloc] initWithURL:url];
    [fetcher startWithCompletionHandler:^(NSData *data){
        _fetchedBarData = data;
    }];
}


// Separate completion & error
#import <Foundation/Foundation.h>

@class EOCNetworkFetcher;
typedef void(^EOCNetworkFetcherCompletionHandler)(NSData *data);
typedef void(^EOCNetworkFetcherErrorHandler)(NSError *error);

@interface EOCNetworkFetcher : NSObject
- (id)initWithURL:(NSURL*)url;
- (void)startWithCompletionHandler:(EOCNetworkFetcherCompletionHandler)completion 
                    failureHandler:(EOCNetworkFetcherErrorHandler)failure;
@end


// Using separate completion & error
EOCNetworkFetcher *fetcher = [[EOCNetworkFetcher alloc] initWithURL:url];
[fetcher startWithCompletionHander:^(NSData *data){
    // Handle success
}
                    failureHandler:^(NSError *error){
    // Handle failure
}];


// Single completion, encapsulating error
#import <Foundation/Foundation.h>

@class EOCNetworkFetcher;
typedef void(^EOCNetworkFetcherCompletionHandler)(NSData *data, NSError *error);

@interface EOCNetworkFetcher : NSObject
- (id)initWithURL:(NSURL*)url;
- (void)startWithCompletionHandler:(EOCNetworkFetcherCompletionHandler)completion;
@end


// Using single completion
EOCNetworkFetcher *fetcher = [[EOCNetworkFetcher alloc] initWithURL:url];
[fetcher startWithCompletionHander:^(NSData *data, NSError *error){
    if (error) {
        // Handle failure
    } else {
        // Handle success
    }
}];


// Progress handler
typedef void(^EOCNetworkFetcherCompletionHandler)(float progress);
@property (nonatomic, copy) EOCNetworkFetcherProgressHandler progressHandler;
