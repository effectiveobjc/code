// Item 49

// Simple example
NSArray *anNSArray = @[@1, @2, @3, @4, @5];
CFArrayRef aCFArray = (__bridge CFArrayRef)anNSArray;
NSLog(@"Size of array = %li", CFArrayGetCount(aCFArray));


// Non-copying keys
#import <Foundation/Foundation.h>
#import <CoreFoundation/CoreFoundation.h>

const void *EOCRetainCallback(CFAllocatorRef allocator, 
                              const void *value)
{
    return CFRetain(value);
}

void EOCReleaseCallback(CFAllocatorRef allocator, 
                        const void *value)
{
    CFRelease(value);
}

CFDictionaryKeyCallBacks keyCallbacks = {
    0,
    EOCRetainCallback,
    EOCReleaseCallback,
    NULL,
    CFEqual,
    CFHash
};

CFDictionaryValueCallBacks valueCallbacks = {
    0,
    EOCRetainCallback,
    EOCReleaseCallback,
    NULL,
    CFEqual
};

CFMutableDictionaryRef aCFDictionary = CFDictionaryCreateMutable(NULL, 0, &keyCallbacks, &valueCallbacks);

NSMutableDictionary *anNSDictionary = (__bridge_transfer NSMutableDictionary*)aCFDictionary;
