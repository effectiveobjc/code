// Item 9

// Class cluster example
typedef NS_ENUM(NSUInteger, EmployeeType) {
    EmployeeTypeDeveloper,
    EmployeeTypeDesigner,
    EmployeeTypeFinance,
};

@interface Employee : NSObject

@property (copy) NSString *name;
@property NSUInteger salary;

// Helper for creating Employee objects
+ (Employee*)employeeWithType:(EmployeeType)type;

// Make an Employee do their respective days work
- (void)doADaysWork;

@end

@implementation Employee

+ (Employee*)employeeWithType:(EmployeeType)type {
    switch (type) {
        case EmployeeTypeDeveloper:
            return [EmployeeDeveloper new];
            break;
        case EmployeeTypeDesigner:
            return [EmployeeDesigner new];
            break;
        case EmployeeTypeFinance:
            return [EmployeeFinance new];
            break;
    }
}

- (void)doADaysWork {
    // Subclasses implement this.
}

@end


// Subclass of the class cluster
@interface EmployeeDeveloper : Employee
@end

@implementation EmployeeDeveloper
- (void)doADaysWork {
    [self writeCode];
}
@end
