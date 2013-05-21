// Item 21

// Throwing exception
id someResource = …;
if ( /* check for error */ ) {
    @throw [NSException exceptionWithName:@"ExceptionName"
                                   reason:@"There was an error"
                                 userInfo:nil];
}
[someResource doSomething];
[someResource release];


// Throwing exception from a method that must be overridden
- (void)mustOverrideMethod {
    @throw [NSException 
        exceptionWithName:NSInternalInconsistencyException
        reason:[NSString stringWithFormat:@"%@ must be overridden", _cmd]
        userInfo:nil];
}


// Returning nil in case of error in initialiser
- (id)initWithValue:(id)value {
    if ((self = [super init])) {
        if ( /* Value means instance can’t be created */ ) {
            self = nil;
        } else {
            // Initialise instance
        }
    }
    return self;
}


// Checking for return error
NSError *error = nil;
BOOL ret = [object doSomethingError:&error];
if (error) {
    // There was an error
}


// Not bothering with the error parameter
BOOL ret = [object doSomethingError:nil];
if (ret) {
    // There was an error
}


// Returning the error
- (BOOL)doSomethingError:(NSError**)error {
    // Do something
    
    NSError *returnError = nil;
    if (/* there was an error */) {
        if (error) {
            *error = [NSError errorWithDomain:domain
                                         code:code
                                     userInfo:userInfo];
        }
        return YES;
    } else {
        return NO;
    }
}


// Defining the error domain and codes
// EOCErrors.h
extern NSString *const EOCErrorDomain;

typedef NS_ENUM(NSUInteger, EOCError) {
    EOCErrorUnknown               = −1,
    EOCErrorInternalInconsistency = 100,
    EOCErrorGeneralFault          = 105,
    EOCErrorBadInput              = 500,
};

// EOCErrors.m
NSString *const EOCErrorDomain = @"EOCErrorDomain";
