// Item 50

// Using NSCache
#import <Foundation/Foundation.h>

// Network fetcher class
typedef void(^EOCNetworkFetcherCompletionHandler)(NSData *data);
@interface EOCNetworkFetcher : NSObject
- (id)initWithURL:(NSURL*)url;
- (void)startWithCompletionHandler:(EOCNetworkFetcherCompletionHandler)handler;
@end

// Class that uses the network fetcher and caches results
@interface EOCClass : NSObject
@end

@implementation EOCClass {
    NSCache *_cache;
}

- (id)init {
    if ((self = [super init])) {
        _cache = [NSCache new];

        // Cache a maximum of 100 URLs
        _cache.countLimit = 100;

        /** 
         * The size in bytes of data is used as the cost, 
         * so this sets a cost limit of 5MB.
         */
        _cache.totalCostLimit = 5 * 1024 * 1024;
    }
    return self;
}

- (void)downloadDataForURL:(NSURL*)url {
    NSData *cachedData = [_cache objectForKey:url];
    if (cachedData) {
        // Cache hit
        [self useData:cachedData];
    } else {
        // Cache miss
        EOCNetworkFetcher *fetcher = [[EOCNetworkFetcher alloc] initWithURL:url];
        [fetcher startWithCompletionHandler:^(NSData *data){
            [_cache setObject:data forKey:url cost:data.length];
            [self useData:data];
        }];
    }
}

@end


// Using NSPurgeableData
- (void)downloadDataForURL:(NSURL*)url {
    NSPurgeableData *cachedData = [_cache objectForKey:url];
    if (cachedData) {
        // Stop the data being purged
        [cacheData beginContentAccess];
        
        [self useData:cachedData];
        
        // Mark that the data may be purged again
        [cacheData endContentAccess];
    } else {
        // Cache miss
        EOCNetworkFetcher *fetcher = [[EOCNetworkFetcher alloc] initWithURL:url];
        [fetcher startWithCompletionHandler:^(NSData *data){
            NSPurgeableData *purgeableData = [NSPurgeableData dataWithData:data];
            [_cache setObject:purgeableData forKey:url cost:purgeableData.length];

            // Don’t need to beginContentAccess as it begins with access already marked
            
            [self useData:data];
            
            // Mark that the data may be purged now
            [purgeableData endContentAccess];
        }];
    }
}
