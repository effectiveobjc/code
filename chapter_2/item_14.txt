// Item 14

// Class hierarchy checking
NSMutableDictionary *dict = [NSMutableDictionary new];
[dict isMemberOfClass:[NSDictionary class]]; ///< NO
[dict isMemberOfClass:[NSMutableDictionary class]]; ///< YES
[dict isKindOfClass:[NSDictionary class]]; ///< YES
[dict isKindOfClass:[NSArray class]]; ///< NO


// Inspecting type of objects in an array during serialisation
- (NSString*)commaSeparatedStringFromObjects:(NSArray*)array {
    NSMutableString *string = [NSMutableString new];
    for (id object in array) {
        if ([object isKindOfClass:[NSString class]]) {
            [string appendFormat:@"%@,", object];
        } else if ([object isKindOfClass:[NSNumber class]]) {
            [string appendFormat:@"%d,", [object intValue]];
        } else if ([object isKindOfClass:[NSData class]]) {
            NSString *base64Encoded = /* base64 encoded data */;
            [string appendFormat:@"%@,", base64Encoded];
        } else {
            // Type not supported
        }
    }
    return string;
}


// Class equality
id object = /* ... */;
if ([object class] == [EOCSomeClass class]) {
    // 'object' is an instance of EOCSomeClass
}