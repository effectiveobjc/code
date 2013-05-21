// Item 10

// C early binding
#import <stdio.h>

void printHello() {
   printf("Hello, world!\n");
}
void printGoodbye() {
    printf("Goodbye, world!\n");
} 

void doTheThing(int type) {
    if (type == 0) {
        printHello();
    } else {
        printGoodbye();
    }
    return 0;
}


// C late binding
#import <stdio.h>

void printHello() {
   printf("Hello, world!\n");
}
void printGoodbye() {
    printf("Goodbye, world!\n");
} 

void doTheThing(int type) {
    void (*fnc)();
    if (type == 0) {
        fnc = printHello;
    } else {
        fnc = printGoodbye;
    }
    fnc();
    return 0;
}
