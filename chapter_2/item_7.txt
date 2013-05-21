// Item 7

// Person interface
@interface Person : NSObject
@property (nonatomic, copy) NSString *firstName;
@property (nonatomic, copy) NSString *lastName;
@property (nonatomic, copy) NSString *fullName; // Convenience for firstName + " " + lastName
@end


// Implementing `fullName' accessors
- (NSString*)fullName {
    return [NSString stringWithFormat:@"%@ %@", 
            self.firstName, self.lastName];
}

- (void)setFullName:(NSString*)fullName {
    NSArray *components = [fullName componentsSeparatedByString:@" "];
    self.firstName = [components objectAtIndex:0];
    self.lastName = [components objectAtIndex:1];
}


// Implementing `fullName' accessors with direct access
- (NSString*)fullName {
    return [NSString stringWithFormat:@"%@ %@", 
            _firstName, _lastName];
}

- (void)setFullName:(NSString*)fullName {
    NSArray *components = [fullName componentsSeparatedByString:@" "];
    _firstName = [components objectAtIndex:0];
    _lastName = [components objectAtIndex:1];
}


// `lastName' setter throwing exception on invalid data
- (void)setLastName:(NSString*)lastName {
    if (![lastName isEqualToString:@"Smith"]) {
        [NSException raise:NSInvalidArgumentException 
                    format:@"Last name must be Smith"];
    }
    self.lastName = lastname;
}


// `brain' lazy initialisation
- (Brain*)brain {
    if (!_brain) {
        _brain = [Brain new];
    }
    return _brain;
}
