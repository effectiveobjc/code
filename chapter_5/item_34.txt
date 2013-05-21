// Item 34

// Switching selector
SEL selector;
if (/* some condition */) {
    selector = @selector(foo);
} else if (/* some other condition */) {
    selector = @selector(bar);
} else {
    selector = @selector(baz);
}
[object performSelector:selector];


// Uh, oh. What memory management semantics to use on `ret'?
SEL selector;
if (/* some condition */) {
    selector = @selector(newObject);
} else if (/* some other condition */) {
    selector = @selector(copy);
} else {
    selector = @selector(someProperty);
}
id ret = [object performSelector:selector];


// Example of calling performSelector:withObject:
id object = /* an object with a property called `value’ */;
id newValue = /* new value for the property */;
[object performSelector:@selector(setValue:) withObject:newValue];
