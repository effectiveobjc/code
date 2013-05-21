// Item 25

// Category on NSString
@interface NSString (HTTP)

// Encode a string with URL encoding
- (NSString*)urlEncodedString;

// Decode a URL encoded string
- (NSString*)urlDecodedString;

@end


// Namespacing the category
@interface NSString (ABC_HTTP)

// Encode a string with URL encoding
- (NSString*)abc_urlEncodedString;

// Decode a URL encoded string
- (NSString*)abc_urlDecodedString;

@end
