// Item 5

enum EOCConnectionState {
    EOCConnectionStateDisconnected,
    EOCConnectionStateConnecting,
    EOCConnectionStateConnected,
};

enum EOCConnectionState state = EOCConnectionStateNotConnected;

enum EOCConnectionState {
    EOCConnectionStateDisconnected,
    EOCConnectionStateConnecting,
    EOCConnectionStateConnected,
};
typedef enum EOCConnectionState EOCConnectionState;

EOCConnectionState state = EOCConnectionStateNotConnected;

enum EOCConnectionStateConnectionState : NSInteger { … };

enum EOCConnectionStateConnectionState : NSInteger;

enum EOCConnectionStateConnectionState {
    EOCConnectionStateDisconnected = 1,
    EOCConnectionStateConnecting,
    EOCConnectionStateConnected,
};

enum EOCMethodOptions {
    EOCMethodOptionOne   = 1 << 0,
    EOCMethodOptionTwo   = 1 << 1,
    EOCMethodOptionThree = 1 << 2,
};

enum EOCMethodOptions options = EOCMethodOptionOne | EOCMethodOptionThree;
if (options & OptionOne) {
    // OptionOne is set
}

typedef NS_ENUM(NSUInteger, EOCConnectionState) {
    EOCConnectionStateDisconnected,
    EOCConnectionStateConnecting,
    EOCConnectionStateConnected,
};
typedef NS_OPTIONS(NSUInteger, EOCMethodOptions) {
    EOCMethodOptionOne = 1 << 0,
    EOCMethodOptionTwo = 1 << 1,
    EOCMethodOptionThree = 1 << 2,
};

typedef enum EOCConnectionState : NSUInteger EOCConnectionState;
enum EOCConnectionState : NSUInteger {
    EOCConnectionStateDisconnected,
    EOCConnectionStateConnecting,
    EOCConnectionStateConnected,
};

typedef enum EOCMethodOptions : int EOCMethodOptions;
enum EOCMethodOptions : int {
    EOCMethodOptionOne = 1 << 0,
    EOCMethodOptionTwo = 1 << 1,
    EOCMethodOptionThree = 1 << 2,
};

EOCMethodOptions options = EOCMethodOptionOne | EOCMethodOptionTwo;

typedef NS_ENUM(NSUInteger, EOCConnectionState) {
    EOCConnectionStateDisconnected,
    EOCConnectionStateConnecting,
    EOCConnectionStateConnected,
};

// …

switch (_currentState) {
    case EOCConnectionStateDisconnected:
        // Handle disconnected state
        break;
    case EOCConnectionStateConnecting:
        // Handle connecting state
        break;
    case EOCConnectionStateConnected:
        // Handle connected state
        break;
}
