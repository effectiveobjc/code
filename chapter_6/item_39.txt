// Item 39

// Block with an int return type
^(BOOL flag, int value){
    if (flag) {
        return value * 5;
    } else {
        return value * 10;
    }
}


// Storing a block to a variable
int (^variableName)(BOOL flag, int value) = ^(BOOL flag, int value){
    // Implementation
}


// Simple typedef
typedef int(^EOCSomeBlock)(BOOL flag, int value);


// Using the typedef
EOCSomeBlock block = ^(BOOL flag, int value){
    // Implementation
};


// Complicated method signature
- (void)startWithCompletionHandler:(void(^)(NSData *data, NSError *error))completion;


// Simplified with typedef
typedef void(^EOCCompletionHandler)(NSData *data, NSError *error);
- (void)startWithCompletionHandler:(EOCCompletionHandler)completion;


// Changing the typedef
typedef void(^EOCCompletionHandler)(NSData *data, NSTimeInterval duration, NSError *error);
