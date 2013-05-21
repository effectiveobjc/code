// Item 38

// Block to add two numbers together
int (^addBlock)(int a, int b) = ^(int a, int b){
    return a + b;
};


// Using the `addBlock'
int (^addBlock)(int a, int b) = ^(int a, int b){
    return a + b;
};
int add = addBlock(2, 5); //< add = 7


// Using the enclosing scope
int additional = 5;
int (^addBlock)(int a, int b) = ^(int a, int b){
    return a + b + additional;
};
int add = addBlock(2, 5); //< add = 12


// __block variables
NSArray *array = @[@0, @1, @2, @3, @4, @5];
__block NSInteger count = 0;
[array enumerateObjectsUsingBlock:^(NSNumber *number, NSUInteger idx, BOOL *stop){
    if ([number compare:@2] == NSOrderedAscending) {
        count++;
    }
}];
// count = 2


// Capturing instance variables
@interface EOCClass
…
- (void)anInstanceMethod {
    // …
    void (^someBlock)() = ^{
        _anInstanceVariable = @"Something";
        NSLog(@"_anInstanceVariable = %@", _anInstanceVariable);
    };
    // …
}
…
@end


// Unsafe!
void (^block)();
if (/* some condition */) {
    block = ^{
        NSLog(@"Block A");
    };
} else {
    block = ^{
        NSLog(@"Block B");
    };
}
block();


// Ah, safe.
void (^block)();
if (/* some condition */) {
    block = [^{
        NSLog(@"Block A");
    } copy];
} else {
    block = [^{
        NSLog(@"Block B");
    } copy];
}
block();
