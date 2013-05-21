// Item 48

// For loops
NSArray *anArray = /* … */;
for (int i = 0; i < anArray.count; i++) {
    id object = anArray[i];
    // Do something with `object’
}

NSDictionary *aDictionary = /* … */;
NSArray *keys = [aDictionary allKeys];
for (int i = 0; i < keys.count; i++) {
    id key = keys[i];
    id value = aDictionary[key];
    // Do something with `key’ and `value’
}

NSSet *aSet = /* … */;
NSArray *objects = [aSet allObjects];
for (int i = 0; i < objects.count; i++) {
    id object = objects[i];
    // Do something with `object’
}


// NSEnumerator
NSArray *anArray = /* … */;
NSEnumerator *enumerator = [anArray objectEnumerator];
while ((id object = [enumerator nextObject]) != nil) {
    // Do something with `object’
}

NSDictionary *aDictionary = /* … */;
NSEnumerator *enumerator = [aDictionary keyEnumerator];
while ((id key = [enumerator nextObject]) != nil) {
    id value = aDictionary[key];
    // Do something with `key’ and `value’
}

NSSet *aSet = /* … */;
NSEnumerator *enumerator = [aSet objectEnumerator];
while ((id object = [enumerator nextObject]) != nil) {
    // Do something with `object’
}


// Reverse NSEnumerator
NSArray *anArray = /* … */;
NSEnumerator *enumerator = [anArray revserseObjectEnumerator];
while ((id object = [enumerator nextObject]) != nil) {
    // Do something with `object’
}


// Fast enumeration
NSArray *anArray = /* … */;
for (id object in anArray) {
    // Do something with `object’
}

NSDictionary *aDictionary = /* … */;
for (id key in aDictionary) {
    id value = aDictionary[key];
    // Do something with `key’ and `value’
}

NSSet *aSet = /* … */;
for (id object in aSet) {
    // Do something with `object’
}


// Reverse fast enumeration
NSArray *anArray = /* … */;
for (id object in [anArray reverseObjectEnumerator]) {
    // Do something with `object’
}


// Block enumeration
NSArray *anArray = /* … */;
[anArray enumerateObjectsUsingBlock:^(id object, NSUInteger idx, BOOL *stop){
    // Do something with `object’
    if (shouldStop) {
        *stop = YES;
    }
}];

NSDictionary *aDictionary = /* … */;
[aDictionary enumerateKeysAndObjectsUsingBlock:^(id key, id object, NSUInteger idx, BOOL *stop){
    // Do something with `key’ and `object’
    if (shouldStop) {
        *stop = YES;
    }
}];

NSSet *aSet = /* … */;
[aSet enumerateObjectsUsingBlock:^(id object, NSUInteger idx, BOOL *stop){
    // Do something with `object’
    if (shouldStop) {
        *stop = YES;
    }
}];


// Casting in the block signature
NSDictionary *aDictionary = /* … */;
[aDictionary enumerateKeysAndObjectsUsingBlock:^(NSString *key,  NSString *object, NSUInteger idx, BOOL *stop){
    // Do something with `key’ and `object’
}];
