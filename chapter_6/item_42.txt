// Item 42

// @synchronized block
- (void)synchronisedMethod {
    @synchronized(self) {
        // Safe
    }
}


// NSLock
_lock = [[NSLock alloc] init];

- (void)synchronisedMethod {
    [_lock lock];
    // Safe
    [_lock unlock];
}


// Synchronised accessors
- (NSString*)someString {
    @synchronized(self) {
        return _someString;
    }
}

- (void)setSomeString:(NSString*)someString {
    @synchronized(self) {
        _someString = someString;
    }
}


// Using GCD queue for synchronisation
_syncQueue = dispatch_queue_create("com.effectiveobjectivec.syncQueue", NULL);

// …

- (NSString*)someString {
    __block NSString *localSomeString;
    dispatch_sync(_syncQueue, ^{
        localSomeString = _someString;
    });
    return localSomeString;
}

- (void)setSomeString:(NSString*)someString {
    dispatch_sync(_syncQueue, ^{
        _someString = someString;
    });
}


// Making the setter an asynchronous dispatch
- (void)setSomeString:(NSString*)someString {
    dispatch_async(_syncQueue, ^{
        _someString = someString;
    });
}


// Using a concurrent queue
_syncQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);

// …

- (NSString*)someString {
    __block NSString *localSomeString;
    dispatch_sync(_syncQueue, ^{
        localSomeString = _someString;
    });
    return localSomeString;
}

- (void)setSomeString:(NSString*)someString {
    dispatch_async(_syncQueue, ^{
        _someString = someString;
    });
}


// Making the concurrent approach work with barriers
_syncQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);

// …

- (NSString*)someString {
    __block NSString *localSomeString;
    dispatch_sync(_syncQueue, ^{
        localSomeString = _someString;
    });
    return localSomeString;
}

- (void)setSomeString:(NSString*)someString {
    dispatch_barrier_async(_syncQueue, ^{
        _someString = someString;
    });
}
