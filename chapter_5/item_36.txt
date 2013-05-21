// Item 36

// Showing how zombies work
#import <Foundation/Foundation.h>
#import <objc/runtime.h>

@interface EOCClass : NSObject
@end

@implementation EOCClass
@end

void PrintClassInfo(id obj) {
    Class cls = object_getClass(obj);
    Class superCls = class_getSuperclass(cls);
    NSLog(@"=== %s : %s ===", class_getName(cls), class_getName(superCls));
}

int main(int argc, char *argv[]) {
    EOCClass *obj = [[EOCClass alloc] init];
    NSLog(@"Before release:");
    PrintClassInfo(obj);
    
    [obj release];
    NSLog(@"After release:");
    PrintClassInfo(obj);
}


// "Deallocating" an object pseudocode with zombies turned on
// Obtain the class of the object being deallocated
Class cls = object_getClass(self);

// Get the class’s name
const char *clsName = class_getName(cls);

// Prepend _NSZombie_ to the class name
const char *zombieClsName = "_NSZombie_" + clsName;

// See if the specific zombie class exists
Class zombieCls = objc_lookUpClass(zombieClsName);

// If the specific zombie class doesn’t exist, then it needs to be created
if (!zombieCls) {
    // Obtain the template zombie class called _NSZombie_
    Class baseZombieCls = objc_lookUpClass("_NSZombie_");

    // Duplicate the base zombie class, where the new class’s name is the prepended string from above
    zombieCls = objc_duplicateClass(baseZombieCls, zombieClsName, 0);
}

// Perform normal destruction of the object being deallocated
objc_destructInstance(self);

// Set the class of the object being deallocated to the zombie class
objc_setClass(self, zombieCls);

// The class of `self’ is now _NSZombie_OriginalClass


// Forwarding mechanism when a zombie is detected
// Obtain the object’s class
Class cls = object_getClass(self);

// Get the class’s name
const char *clsName = class_getName(cls);

// Check if the class is prefixed with _NSZombie_
if (string_has_prefix(clsName, "_NSZombie_") {
    // If so, this object is a zombie
    
    // Get the original class name by skipping past the _NSZombie_, i.e. taking the substring from character 10
    const char *originalClsName = substring_from(clsName, 10);
    
    // Get the selector name of the message
    const char *selectorName = sel_getName(_cmd);
    
    // Log a message to indicate which selector is being sent to which zombie
    Log("*** -[%s %s]: message sent to deallocated instance %p", originalClsName, selectorName, self);
    
    // Kill the application
    abort();
}


// Showing zombies in action
EOCClass *obj = [[EOCClass alloc] init];
NSLog(@"Before release:");
PrintClassInfo(obj);

[obj release];
NSLog(@"After release:");
PrintClassInfo(obj);

NSString *desc = [obj description];
