// Item 28

// Database connection protocol
@protocol EOCDatabaseConnection
- (void)connect;
- (void)disconnect;
- (BOOL)isConnected;
- (NSArray*)performQuery:(NSString*)query;
@end


// Database manager class
#import <Foundation/Foundation.h>

@protocol EOCDatabaseConnection;

@interface EOCDatabaseManager : NSObject
+ (id)sharedInstance;
- (id<EOCDatabaseConnection>)connectionWithIdentifier:(NSString*)identifier;
@end


// Fetched results controller with section info
NSFetchedResultsController *controller = /* some controller */;
NSUInteger section = /* section index to query */;

NSArray *sections = controller.sections;
id <NSFetchedResultsSectionInfo> sectionInfo = sections[section];
NSUInteger numberOfObjects = sectionInfo.numberOfObjects;
