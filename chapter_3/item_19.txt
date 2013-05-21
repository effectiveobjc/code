// Item 19

// Replacing string with string in string
NSString *text = @"The quick brown fox jumped over the lazy dog";
NSString *newText = [text stringByReplacingOccurrencesOfString:@"fox" withString:@"cat"];


// C++ example of naming
class Rectangle {
public:
    Rectangle(float width, float height);
    float getWidth();
    float getHeight();
private:
    float width;
    float height;
};

Rectangle *aRectangle = new Rectangle(5.0f, 10.0f);


// Objective-C equivalent
#import <Foundation/Foundation.h>

@interface EOCRectangle : NSObject

@property (nonatomic, assign, readonly) float width;
@property (nonatomic, assign, readonly) float height;

- (id)initWithSize:(float)width :(float)height;

@end

EOCRectangle *aRectangle = 
    [[EOCRectangle alloc] initWithSize:5.0f :10.0f];


// Better initialiser name
- (id)initWithWidth:(float)width andHeight:(float)height;
EOCRectangle *aRectangle = 
    [[EOCRectangle alloc] initWithWidth:5.0f andHeight:10.0f];


// Well named methods
- (EOCRectangle*)unionRectangle:(EOCRectangle*)rectangle
- (float)area


// Badly named methods
- (EOCRectangle*)union:(EOCRectangle*)rectangle // Unclear
- (float)calculateTheArea // Too verbose
