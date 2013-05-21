// Item 46

// GCD locked accessors
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


// Attempting to fix the problem of `dispatch_sync' maybe blocking
- (NSString*)someString {
    __block NSString *localSomeString;
    dispatch_block_t accessorBlock = ^{
        localSomeString = _someString;
    };

    if (dispatch_get_current_queue() == _syncQueue) {
        accessorBlock();
    } else {
        dispatch_sync(_syncQueue, accessorBlock);
    }

    return localSomeString;
}


// Explaining deadlock
dispatch_queue_t queueA = dispatch_queue_create("com.effectiveobjectivec.queueA", NULL);
dispatch_queue_t queueB = dispatch_queue_create("com.effectiveobjectivec.queueB", NULL);

dispatch_sync(queueA, ^{
    dispatch_sync(queueB, ^{
        dispatch_sync(queueA, ^{
            // Deadlock
        });
    });
});


// Explaining why checking the current queue does not always fix the deadlock
dispatch_sync(queueA, ^{
    dispatch_sync(queueB, ^{
        dispatch_block_t block = ^{ /* … */ };
        if (dispatch_get_current_queue() == queueA) {
            block();
        } else {
            dispatch_sync(queueA, block);
        }
    });
});


// Queue specific data example
dispatch_queue_t queueA = dispatch_queue_create("com.effectiveobjectivec.queueA", NULL);
dispatch_queue_t queueB = dispatch_queue_create("com.effectiveobjectivec.queueB", NULL);
dispatch_set_target_queue(queueB, queueA);

static int kQueueSpecific;
CFStringRef queueSpecificValue = CFSTR("queueA");
dispatch_queue_set_specific(queueA, 
                            &kQueueSpecific, 
                            (void*)queueSpecificValue, 
                            (dispatch_function_t)CFRelease);

dispatch_sync(queueB, ^{
    dispatch_block_t block = ^{ NSLog(@"No deadlock!"); };
    
    CFStringRef retrievedValue = dispatch_get_specific(&kQueueSpecific);
    if (retrievedValue) {
        block();
    } else {
        dispatch_sync(queueA, block);
    }
});
